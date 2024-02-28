---
author: "Abhilash"
title: "The power of interfaces"
date: "2024-02-28"
description: "Describes how interfaces enable dynamic plugging in of functionality aka runtime polymorphism"
tags: ["C-sharp", "interfaces", "polymorphism"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

### Prologue
Some time back I took a session on C# & SOLID for a set of new hires to our [Org](https://carestack.com/). As a part of explaining `types in C#` we also looked into interfaces. I recalled how I had difficulty in understanding the power of interfaces when I was a new bean like them and how over the years it dawned on me that how this simple looking type is so powerful that it drives or enables a plugin or component driven architecture. 

So this post is an attempt explain how interfaces go about doing the same.

### IComparable\<T\>
We can try to understand what interfaces achieve by looking at how one of the standard library interfaces, like IComparable\<T\>, works.

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

Here the code is to sort a list of `Person` objects, if we run this as is we get an exception.

```
System.InvalidOperationException: 'Failed to compare two elements in the array.'
Inner Exception
ArgumentException: At least one object must implement IComparable.
```

What this error message is trying to say is that the program tried to sort the `List<Person>` using `List<T>.Sort()` method but since ultimately all sorting algorithms require some form of comparison between two elements, the program wasnt able to sort `List<Person>`, because it doesn't know how to compare a single person object to another person object within the list.

So as the error message suggests we will implement the `IComparable` interface as follows
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

So looking at the code one should be able to correlate as to how `IComparable<Person>` solves the problem. It defines a logic to compare two different instances of Person objects in the `CompareTo` method. In this case the comparison happens using the Age property, but in fact the comparison logic can be anything the developers desire or rather what the business logic dictates.

### Polymorphism
During my initial years this behavior of `IComparable<T>` almost fell like magic, I mean to sort any custom type, say `Dog`, all we need to do is implement `IComparable<Dog>` and to do the same with `Cat` implement `IComparable<Cat>`, and sort `Cats` and `Dogs`, Now how cool is that :).

But how does this thing really work under the hood ? How does the program locate and sort of plugin the right `IComparable<Person>` implementation when it is trying to do `List<Person>.Sort()`. The answer lies in polymorphism, specifically dynamic polymorphism in this case.

If we were to understand this with a visual analogy, it is like a torch being covered with a colored film, the torch light is as such colorless usually, but if covered by different colored films will produce lights of that color which is a dynamic polymorphic behavior, totally unpredictable until a film of certain color is covering the light source.

{{<figure src="images/ColoredTorches.png" >}}

To understand this in more detail we will have to look at the `List<T>.Sort()` implementation and see how it uses the `CompareTo` method on each pair of objects dynamically.

I tried to find the implementation of `List<T>.Sort()` on GitHub but it seemed like a [rabbit hole](https://github.com/microsoft/referencesource/blob/master/mscorlib/system/collections/generic/list.cs#L949), so lets try to understand whats happening under the hood with some code of our own.

So lets implement a custom sorting function or method called `CustomSort<T>` so that we can call `List<T>.CustomSort()` and use `IComparable<T>` within it to complete the implementation of `CustomSort`, this is how the code would look like.

```
public static class ListExtensions
{
    public static void CustomSort<T>(this List<T> list) where T : IComparable<T> // 1. Constrain T to be a type of IComparable<T>
    {
        if (list == null)
        {
            throw new ArgumentNullException(nameof(list));
        }

        // Using Bubble Sort as an example
        for (int i = 0; i < list.Count - 1; i++)
        {
            for (int j = 0; j < list.Count - 1 - i; j++)
            {
                if (list[j].CompareTo(list[j + 1]) > 0) // 2. list[j], list[j+1] are both of type T hence implement IComparable<T>, hence we can call Compare on them
                {
                    // Swap elements if they are in the wrong order
                    T temp = list[j];
                    list[j] = list[j + 1];
                    list[j + 1] = temp;
                }
            }
        }
    }
}

```
As mentioned in comment 1. this line,

```
public static void CustomSort<T>(this List<T> list) where T : IComparable<T>
```
means that we are defining an [extension method](https://www.geeksforgeeks.org/extension-method-in-c-sharp/) on `List<T>` and we are stating a [generic](https://www.programiz.com/csharp-programming/generics) constraint `where T : IComparable<T>` means that this method `CustomSort` can only be invoked by a `List` of a type `T` where `T` implements `IComparable`.

So `List<Person>` can call `CustomSort` only if `Person` implements `IComparable<Person>`, otherwise we get this compile time error, this is a bit different from how .NET framework library has done this in the sense it is checking only at run time for an `IComparable<Person>` implementation.

So if we do the below without `Person` implementing `IComparable`

```
// Create a list of Person objects
List<Person> people = new List<Person>
{
    new Person { Name = "Alice", Age = 30 },
    new Person { Name = "Bob", Age = 25 },
    new Person { Name = "Charlie", Age = 35 }
};

// Use the List.CustomSort method, which internally uses IComparable<T>
people.CustomSort();
```

We get this.

```
The type 'Person' cannot be used as type parameter 'T' in the generic type or method 'ListExtensions.CustomSort<T>(List<T>)'. There is no implicit reference conversion from 'Person' to 'System.IComparable<Person>'.
```

by which the compiler means that `List<T>.CustomSort` cant be invoked by any `T` in this case `Person` which is not implementing `IComparable<T>`.

And so if we implement `IComparable<Person>` in `Person` as below, everything falls into place, and sorting just works because in the actual comparison in the sorting algorithm, the implementation going to be used here is `IComparable<Person>.CompareTo`

```
if (list[j].CompareTo(list[j + 1]) > 0) 

// list[j], list[j+1] are both of type Person hence implement IComparable<Person>, hence we can call CompareTo on them
```
So with `IComparable<T>` we sort of plugin a certain implementation of `T` it could be `Person` or `int` and the right implementation will be invoked here based on which `List<T>` i.e. `List<Person>` or `List<int>` is calling the sorting function, as shown in the below visual.

{{<figure src="images/ICompPlugin.png" >}}


Here is the final version of the code, I would recommend debugging through this code to understand it better.

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

// Create a list of Person objects
List<Person> people = new List<Person>
{
    new Person { Name = "Alice", Age = 30 },
    new Person { Name = "Bob", Age = 25 },
    new Person { Name = "Charlie", Age = 35 }
};

// Use the List.CustomSort method, which internally uses IComparable<T>
people.CustomSort();

// Display the sorted list
foreach (var person in people)
{
    Console.WriteLine($"{person.Name} - {person.Age} years old");
}

```

this would yield the same result as using `List<T>.Sort`.

```
Bob - 25 years old
Alice - 30 years old
Charlie - 35 years old
```

### Additional Stuff
Those of you interested in a bit more knowledge. Lets again look at this code.

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


notice the `return Age.CompareTo(other?.Age);` Lets point the cursor on this and see what the documentation says

{{<figure src="images/CompareToIntDef.png" >}}

This is interesting because here the code `return Age.CompareTo(other?.Age);` actually is an implementation of `IComparable<int>` which has the expected return value int ( more on it a bit later).

Which means that our `IComparable<Person>.CompareTo(Person? other)` is infact depending on `IComparable<int>.CompareTo(Object? value)` and that implies the `int` type too is implementing an `IComparable<int>`, as is the case when we look deeper into `Int32`'s defintion.

{{<figure src="images/CompareToIntImpl.png" >}}

 
Now coming back to the `int` return value of `IComparable<T>`, if the int returned is `Less than zero` then it is interpreted as the current instance being less than incoming instance. 

{{<figure src="images/ReturnVal1.png" >}}

If `Zero` then both values are equal.

{{<figure src="images/ReturnVal3.png" >}}

if `Greater than zero` then incoming value is less than the current object where the `IComparable` is implemented.

{{<figure src="images/ReturnVal2.png" >}}

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

from this we should understand that somewhere in the .NET framework library code  an implementation of 

```
IComparable<int>.CompareTo(Object? value)
``` 

must be doing something roughly(not exactly) like this 

```
return this.value < (int)value ? -1 : this.value == (int)value ? 0 : 1;
```

