---
author: "Abhilash"
title: "The power of interfaces"
date: "2023-09-27"
description: "Describes how interfaces enable plugging in functionality dynamically aka runtime polymorphism"
tags: ["C#", "interfaces", "polymorphism"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

### Prologue
Couple of weeks ago I took a session on C# & SOLID for a set of new joiness to our [Org](https://carestack.com/). As a part of explaining types in C# we chanced upon interfaces. I reiterated to them how I had difficulty in understanding the power of interfaces when I was a new bean and how over the years it dawned on me that how this simple looking type, drives or enables a plugin or component driven architecture. 

So this post which is more suited to those who are starting out in C#, is an attempt to explain how interfaces go about doing the same.

### IComparable\<T\>
We can explain what interfaces achieve by looking at how one of the standard library interfaces, which is IComparable\<T\>, works.
So lets look at some code.

```
public class Person
{
    public string? Name { get; set; }

    public int Age { get; set; }
}
```
```
// Create a list of Person objects
List<Person> people = new List<Person>
{
    new Person { Name = "Alice", Age = 30 },
    new Person { Name = "Bob", Age = 25 },
    new Person { Name = "Charlie", Age = 35 }
};

// Use the List.Sort method, which internally uses IComparable<T>
people.Sort();

// Display the sorted list
foreach (var person in people)
{
    Console.WriteLine($"{person.Name} - {person.Age} years old");
}
```

We are trying to sort a list of Person objects here, if we run this code as is we get an exception.

```
System.InvalidOperationException: 'Failed to compare two elements in the array.'
Inner Exception
ArgumentException: At least one object must implement IComparable.
```

What this error message is trying to say is that the program tried to sort the `List<Person>` using `List<T>.Sort()` method but since ultimately all sorting algorithms require some form of comparison of two elements, the program wasnt able to do so, because it doesnt know how or rather on what basis to compare a single person object to another person object.

So as the error message suggests we implement the IComparable interface as follows
```
public class Person : IComparable<Person>
{
    public string Name { get; set; }
    public int Age { get; set; }

    // Implement the CompareTo method from the IComparable<T> interface
    public int CompareTo(Person? other)
    {
        // Compare persons based on their age
        return Age.CompareTo(other?.Age);
    }
}
```

And now on running the program we get the following output

```
Bob - 25 years old
Alice - 30 years old
Charlie - 35 years old
```

So one should be able to correlate as to how IComprable solves the problem by defining a logic to compare two different instances of Person objects in the `CompareTo` method. In this case the comparison happens using the Age property, but in fact the comparsion logic can be anything the developers desire or the business logic dictates.

Also notice the `return Age.CompareTo(other?.Age);` Lets point the cursor on this and see what the documentation says

{{<figure src="images/CompareToIntDef.png" >}}

With this description we come to know that our `IComparable<Person>.CompareTo(Person? other)` is infact depending on `IComparable<int>.CompareTo(Object? value)` which means somewhere the `int` type too is implementing an `IComparable<int>`, as is the case when we look deeper into `int`'s defintion.

{{<figure src="images/CompareToIntImpl.png" >}}

More interesting is the fact that emerges from the `return Age.CompareTo(other?.Age);` defintion. It is the expected return value's (int) interpretation. 

If the int returned is `Less than zero` then it is interpreted as the current instance being less than incoming instance. If `Zero` then both values are equal & if `Greater than zero` then incoming value is less than the current object where the IComprabale is implemented.

This also means that `IComparable<Person>.CompareTo(Person? other)` can be rewritten without relying on `IComparable<int>.CompareTo(Object? value)` with the same results.

```
public class Person : IComparable<Person>
{
    public string Name { get; set; }
    public int Age { get; set; }

    // Implement the CompareTo method from the IComparable<T> interface
    public int CompareTo(Person? other)
    {
        // Compare persons based on their age
        return Age < other.Age ? -1 : Age == other.Age ? 0 : 1;
    }
}
```

will also lead to

```
Bob - 25 years old
Alice - 30 years old
Charlie - 35 years old
```

### Polymorphism
