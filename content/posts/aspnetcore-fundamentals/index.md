---
author: "Abhilash"
title: "ASP.NET Core fundamentals overview"
date: "2021-09-24"
description: "Quick summary on fundamentals of asp.net core"
tags: ["asp.net core"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

The article describes some of the fundamentals of asp.net core apps knowing which might be the key to using the framework in the best possible way.

### The Startup class
The startup class as the name indicates is the starting point of the app which comes into playing when the app starts to run or handles a request, there are two methods of importance in the startup class
1. ConfigureServices
   
   This method is called by the ASP.NET Core runtime when the app starts to configure services(reusable components/types) needed in the next Configure method and further down the app lifecycle during request processing etc.

2. Configure

    ASP.NET Core request processing pipeline is built using middlewares which are nothing but components that directly working on the incoming request using the HttpContext. So this is the method called by the run time during app startup and this is where the request processing pipeline is built aka the middlewares are configured.

For more information, see [App startup in ASP.NET Core]().

### Dependency injection
