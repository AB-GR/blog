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
ASP.NET Core has a built in DI framework for providing configured services across the  app. Services are configured in `StartUp.ConfigureSerices` method like how a efcore dbcontext is configured below for consumption by the app code.

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<RazorPagesMovieContext>(options => 
    options.UseSqlServer(Configuration.GetConnectionString("RazorPagesMovieContext")));

    services.AddControllersWithViews();
    services.AddRazorPages();
}
```
And the services can be consumed via constructor injection by declaring a type or an interface implementing a type needed, as a constructor parameter. Below we are accessing the db context declared in the previous block of code in a controller.

```
public class IndexModel : PageModel
{
    private readonly RazorPagesMovieContext _context;

    public IndexModel(RazorPagesMovieContext context)
    {
        this._context = context;
    }

    // ...

    public async Task OnGetAsync()
    {
        Movies = await _context.Movies.ToListAsync();
    }
}
```

If for some reason the built in IOC does not meet all the app's requirements then a third party IOC also can be used.

### Middleware
The ASP.NET Core request processing pipeline is composed of a series of middleware components. Each such component performs operation on an `HttpContext` and then either calls the next middleware component or terminates the request. ASP.NET Core provides a rich set of inbuilt middleware and we can also write custom middleware to suit our needs. By default a middleware component is added to the request processing pipeline by invoking the `Use..` extension from the `StartUp.Configure` method. For example to render processing of static files call `UseStaticFiles` as shown below.

```
public void Configure(IApplicationBuilder app)
{
    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapDefaultControllerRoute();
        endpoints.MapRazorPages();
    });
}
```

### Host
When the ASP.NET Core app starts up it builds a host, which is an encapsulation of all resourecs available to the app which includes
1. An http server implementation
2. Middleware components
3. Configuration
4. DI services
5. Logging

There are two kinds of hosts
1. .NET Generic host
2. ASP.NET Core Web host

No 1. is recommended as No 2. exists for a backward compatibility purposes when migrating from asp.net core 2.x apps

The following example creates a .NET Generic host

```

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args).ConfigureWebHostDefaults(webBuilder => {
            webBuilder.UseStartUp<StartUp>();
      });
}

```

The `CreateDefaultBuilder` & `ConfigureWebHostDefaults` methods do the following
1. Use Kestrel as web server and enable IIS integration.
2. Load settings from appsettings.json, appsettings.{environment}.json, environment variables, command line args and other configuration sources.
3. Send logging output to Console and other debug providers.

The Generic Host helps in non web apps such as background tasks use cross cutting functionality/services such as DI, Logging, Configuration & app lifetime managment.

### Server
ASP.NET core app runs on a server which is an http server implementation and responds to http requests. The request is surfaced to an app as HttpContext object which is basically a composition of RequestFeature interfaces.

On windows platform the server can a kestrel server, IIS (in proc), http.sys, on mac OS & linux it is a kestrel server.

kesterel is a cross platform web server implementation provided by ASP.NET Core and can run independently as an edge server or can run in reverse proxy configuration with native server implementations like IIS, Nginx, apache etc.

### Configuration
ASP.NET Core provides a configuration framework that supports reading from multiple sources/configuration providers such as .json files, .xml files, environment variables, command line args and all these settings are ultimately available as name value pairs in the app, there are built in configuration providers that do the job but we can always create a custome one. 

After all configuration settings are loaded the settings from environment variables override the same name settings from appsettings.json

The preferred way to related configuration values is using the options pattern.

Confidential configuration such as passwords are managed using secret manager while secrets in production can be managed using azure key vault.

### Environments
Execution environments such as development, staging, production are supported out of the box in ASP.NET  Core. We can specify the environment the app is running in by setting the  ASPNETCORE_ENVIRONMENT variable ASP.NET Core reads the environment varibale at startup and stores the value in IWebHosEnvironment service which is available throughout the app via DI.

The following example configures the app to set detailed error information in case of a Development environment

```
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }
```

### Logging
ASP.NET Core logging supports multiple logging providers like
1. Console
2. Debug
3. Trace Events
4. Windows Event Log
5. Trace Source
6. App Service
7. App  Insights

To log first resolve a ILogger<TCategoryName> via DI and call the LogInformation method, this method accepts any number of characters which most of the time is used to prepare the error message by some logging providers use the parameters to implement strucutred or semantic logging

### Routing
A route is a url pattern that is mapped to a handler. A handler may be a razor page, an action method or a middleware. ASP.NET Core routing gives you control over the urls used by your app.

### Error handling
ASP.NET Core has following features
1. A developer exception page
2. Custom error pages
3. Static status code pages
4. Startup exception handling

### Content root
Content root is the base path for
1. Host exe hosting the app
2. The app dll
3. Content files used by the  app such as 
    Razor files (.cshtml, .razor), Config files (.json, .xml), Data files(.db)
4. the web root (wwwroot)

### Web root
Web root is the base path for static content in wwwroot such as 
1. Stylesheet (css)
2. Javascript (.js)
3. Images (.png, .jpg)
