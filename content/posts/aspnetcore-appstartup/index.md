---
author: "Abhilash"
title: "ASP.NET Core App startup"
date: "2021-09-30"
description: "More on asp.net core startup"
tags: ["asp.net core"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

### The Startup class
ASP.NET Core uses the the startup class which is named so by convention, on that a bit later, so this class consists of two methods ConfigureServices and Configure. ConfigureServices as we know is used to setup all services into the DI so that they are accessible across the app and Configure method is used to setup the app behavior in response to a request in the form of a chain of middlewares.

The StartUp class is specified when app's host is built by calling `WebHostBuilderExtensions.UseStartup<TStartup>`

```
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```