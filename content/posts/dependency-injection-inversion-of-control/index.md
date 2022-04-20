---
author: "Abhilash"
title: "Dependency Injection and Inversion of Control"
date: "2021-09-20"
description: "Trying to explain that dependency injection is IOC in action"
tags: ["Architectural principles"]
ShowToc: false
ShowBreadCrumbs: false
draft: true
---

We have heard these keywords of DI & IOC being used when it comes to discussing modular and loosely coupled application components. So what are these terms and how are they related ?

IOC or inversion of control or dependency inversions is a principle which states that the direction of dependency between objects/components within an application should be in the direction of an abstraction rather than an implementation. lets examine what this means.

Say we have a class A which has a method which calls a method on class B and this class has a method that calls another method on class C, so class A references class B which in turn references class C, this is called a direct dependency where at compile time and run time the classes higher in the call stack depend on the lower ones, a diagramatic representation would look something like this.

{{<figure src="images/ioc1.png" >}}

Now there are obvious problems with this tight coupling such as difficulty in testing, difficulty in reuse, possible breaking changes with version upgrades if the consumed class happens to reside in a library not in the consumers control, also the technical debt imposed with this rigidity will become hard to pay as the system complexity increases and it becomes no longer upgradeable with changing requirements.

In such situations its always better to depend on an abstraction or an interface as it removes the headache of dependency lifecycle management from the consumer whereby the consumer just states his demands or requirements in the form of an abstraction/contract/interface and the dependency management container is responsible to supply it at run time. However simplifying the life of the consumer code does come at a cost in terms of use of containers and its boiler plate code but that is a trade off we need to take in order to achieve stability in the application over the long term

A small real life example will make this simpler to understand. Imagine seeking a service of a helper to take care of your child, now a lot depends on the quality service provided by the helper in terms of trustworthiness, reliability etc for you should be able to derive the maximum benefit from such a service, now imagine if there are issues in satisfying these expectations at helpers end then you'd mostly be in a firefighting mode by say having to call a day off work when the helper didnt show up. These issues at a high level can be attributed to the tight coupling between you and the helper.

One way to solve this problem is to abstract away the helpers service to a service provider agency. With an agency you provide your requirments and expectation in terms of quality of service and it is then their responsibility to satisfy it for continuity of service and they'd highly likely do so by finding the right fit for the job. Ofcourse all of these guarantees in service raises the cost of the service with the middleman service provider taking a cut from the service cost, but then that is trade off you need to decide upon if you value peace of mind over cost.

So when we make the consumer, in this case class A, depend on an abstraction of class B then we say that compile time direction of dependency is inverted because the class B has to now implement the abstraction in order to be of some use to class A rather the other way around which was basically a take it or leave it approach by class B. Hence this approach is also called Inversion of Control, in our helper analogy this is akin to the helper coming to terms with the requirements of the service consumer via the service provider agency.

{{<figure src="images/ioc2.png" >}}

Dependency injection is one of the patterns to achieve inversion of control the other being service locator pattern.
It usually involves a container which is responsible for providing all the dependcies needed by different types in the application and it does this via 3 ways

1. Constructor injection
2. Property injection
3. Interface injection

the container is also responsible for dependency configuration at the app/process startup

Some more links that explain this concept very well.

https://martinfowler.com/articles/injection.html#InversionOfControl
https://stackoverflow.com/a/3311657/6024150