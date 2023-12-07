---
author: "Abhilash"
title: "What is a C\# Record"
date: "2022-07-31"
description: "What are C\# records and what problem do they solve"
tags: ["c-sharp", "record"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

## Background
Writing immutable types is a popular strategy for simplifying software and reducing bugs. In C# the `Record` type solves the following 3 issues/code bloats associated with writing immutable types otherwise.

### Language supported pattern for indestructible mutation
Modification of an immutable type can be done by copying over its values while incorporating the changes, now this is not as inefficient as it sounds if we consider shallow copy(opposite of deep copy where you copy over sub objects and collections). But in terms of coding effort, doing this for each and every property can be very inefficient. Records solve this problem via a language supported pattern.

```
record Point
{
  public Point (double x, double y) => (X, Y) = (x, y);

  public double X { get; init; }
  public double Y { get; init; }    
}
```

The preceding record declaration expands into something like this:

```
class Point
{  
  public Point (double x, double y) => (X, Y) = (x, y);

  public double X { get; init; }
  public double Y { get; init; }    

  protected Point (Point original)    // “Copy constructor”
  {
    this.X = original.X; this.Y = original.Y
  }

  // This method has a strange compiler-generated name:
  public virtual Point <Clone>$() => new Point (this);   // Clone method

  // Additional code to override Equals, ==, !=, GetHashCode, ToString()
  // ...
}
```

As you can see the most important step that the compiler performs with all records is to write a copy constructor (and a hidden Clone method). This enables nondestructive mutation via the with keyword:

```
Point p1 = new Point (3, 3);
Point p2 = p1 with { Y = 4 };
Console.WriteLine (p2);       // Point { X = 3, Y = 4 }

record Point (double X, double Y);
```
In this example, p2 is a copy of p1, but with its Y property set to 4. The benefit is more apparent when there are more properties:

```
Test t1 = new Test (1, 2, 3, 4, 5, 6, 7, 8);
Test t2 = t1 with { A = 10, C = 30 };
Console.WriteLine (t2);

record Test (int A, int B, int C, int D, int E, int F, int G, int H);
```

Here’s the output:

```
Test { A = 10, B = 2, C = 30, D = 4, E = 5, F = 6, G = 7, H = 8 }
```

Nondestructive mutation occurs in two phases:
 - First, the copy constructor clones the record. By default, it copies each of the record’s underlying fields, creating a faithful replica while bypassing (the overhead of) any logic in the init accessors. All fields are included (public and private, as well as the hidden fields that back automatic properties).
 - Then, each property in the member initializer list is updated (this time using the init accessors).

The compiler translates:

```
Test t2 = t1 with { A = 10, C = 30 };
```
into

```
Test t2 = new Test(t1);  // Use copy constructor to clone t1 field by field
t2.A = 10;               // Update property A
t2.C = 30;               // Update property C
```
### Quick create of POCO objects
Its always useful to create a type which only has data and no behavior, esp in functional programming, but conventionally creating such types is more work than it should be like defining a class, constructors and properties. Records simplify this.

When you define a record with a parameter list the compiler takes care of creating the property declarations, a primary constructor and deconstructor. Below is a very simple way of creating a Student entity, the properties for ID, first name and last name are created automatically by the compiler.

```
public record Student(string ID, string LastName, string GivenName)
{
}
```

### Equality checks
In case of immutable types we only need to check for structural similarity as identity wise the object will not change. Records give you structural equality by default, regardless of underlying type is a class or struct, without any boilerplate code.

Just as with structs, anonymous types, and tuples, records provide structural equality out of the box, meaning that two records are equal if their fields (and automatic properties) are equal:

```
var p1 = new Point (1, 2);
var p2 = new Point (1, 2);
Console.WriteLine (p1.Equals (p2));   // True

record Point (double X, double Y);
```

The equality operator also works with records (as it does with tuples):

```
Console.WriteLine (p1 == p2);         // True
```

The default equality implementation for records is unavoidably fragile. In particular, it breaks if the record contains lazy values, transient values, arrays, or collection types (which require special handling for equality comparison). Fortunately, it’s relatively easy to fix (should you need equality to work), and doing so is less work than adding full equality behavior to classes or structs.

Unlike with classes and structs, you do not (and cannot) override the object.Equals method; instead, you define a public Equals method with the following signature:

```
record Point (double X, double Y)
{
  double _someOtherField;
  public virtual bool Equals (Point other) =>
    other != null && X == other.X && Y == other.Y;
}
```

The Equals method must be virtual (not override), and it must be strongly typed such that it accepts the actual record type (Point in this case, not object). Once you get the signature right, the compiler will automatically patch in your method.

In our example, we changed the equality logic such that we compare only X and Y (and ignore _someOtherField).