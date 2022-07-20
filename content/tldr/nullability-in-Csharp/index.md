---
author: "Abhilash"
title: "Nullability in C# 10"
date: "2022-07-19"
description: "Brief overview of nullability in C# 10 what it is and what it isnt"
tags: ["asp.net core", "rate-limiting"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

### Source Article(s)
https://jeremybytes.blogspot.com/2022/07/nullability-in-c-what-it-is-and-what-it.html

### Nullable reference type setting 
This setting is enabled by default in a .NET 6 project

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>

```

What this means is C# reference types will be treated as Non nullable by default. Does this mean the below code wont compile.

```
string s = null;

```

No. but it shows up this warning.

 {{<figure src="images/nicsharp1.png" >}}

This message is trying to tell us, hey you say that string `s` is `not` expected to be null but here you are assigning it to null.

Now what if we mark it as `nullable` and use it in the next statement.

```
string? s = null;

Console.WriteLine(s.Replace("c", "a"));
```

Then we see this warning.

{{<figure src="images/nicsharp2.png" >}}

Compiler is trying to warn us of a possible NRE, So now we need to implement guard clauses wherever `s` is to be used or ensure it is initialized with a `non-null` value and continues to be `non-null` throught its usage in the application.

```
string? s = null;

if(!string.IsNullOrWhiteSpace(s))
	Console.WriteLine(s.Replace("c", "a"));
```

### What this means is 

*Nullability Is*:

- A way to get compile-time warnings about possible null references
- A way to make the intent of your code more clear to other developers

*Nullability Is NOT*:

- A way to prevent null reference exceptions at runtime
- A way to prevent someone from passing a null to your method or assigning a null to an object



