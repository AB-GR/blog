---
author: "Abhilash"
title: "Dependency Injection and Inversion of Control"
date: "2021-09-20"
description: "Trying to explain that dependency injection is IOC in action"
tags: ["Architectural principles"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

We have heard these keywords of DI & IOC being used when it comes to discussing modular and loosely coupled application components. So what are these terms and how are they related ?

IOC or inversion of control or dependency inversions is a principle which states that the direction of dependency between objects/components within an application should be in the direction of an abstraction rather than an implementation. lets examine what this means.

Say we have a class A which has a method which calls a method on class B and this class has a method that calls another method on class C, so class A references class B which in turn references class C, this is called a direct dependency where at compile time and run time the classes higher in the call stack depend on the lower ones, a diagramatic representation would look something like this.

{{<figure src="images/ioc1.png" >}}

Now there are obvious problems with this tight coupling 

https://martinfowler.com/articles/injection.html#InversionOfControl
https://stackoverflow.com/questions/3058/what-is-inversion-of-control