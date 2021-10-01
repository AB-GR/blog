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

### The ConfigureServices method
The `ConfigureServices` method is
1. Optional
2. Called before the `Configure` method
3. Where configuration options are set by convention

For features that require substantial setup, there are `Add{Service}` extension methods on IServiceCollection such as AddDbContext, AddStaticFiles, AddRazorPages, AddControllersWithViews, AddDefaultIdentity, AddEntityFrameworkStores etc.

Add such services to service collection makes them available in the App as well as the `Configure` method.

### The Configure method
The `Configure` method configures the middleware which creates the request pipeline which determines how the app responds to http requests. This method is injected with an instance of `IApplicationBuilder` on which the `Use` extension methods are called which configure one or more middleware. The middleware executes some logic on HttpContext and then calls the next middleware component or shortcircuits the call if appropriate.

The ASP.NET Core templates for MVC & such come with prebuilt `Configure` method code which provide `Use` extensions to
1. Set up developer exception page
2. Exception handling
3. HSTS
4. Http redirection
5. Static files
6. Setup up MVC or Razor pages

Apart from an instance of `IApplicationBuilder` the  `Configure` method also exposes `IWebHostEnvironment` & `ILoggerFactory` it can also include in its signature any service that has been configured in `ConfigureServices` method.