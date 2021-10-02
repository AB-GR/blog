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

### Configure services without Startup
The methods `Configure` and `ConfigureServices` can be called on the host builder as in the `CreateHostBuilder` method shown below, thereby negating the  need of a separate StartUp class. These methods are convenience methods and can be chained up but calls to multiple `ConfigureServices` get appended and in case of multiple calls to `Configure` on the last one gets called.

```
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
            })
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.ConfigureServices(services =>
                {
                    services.AddControllersWithViews();
                })
                .Configure(app =>
                {
                    var loggerFactory = app.ApplicationServices
                        .GetRequiredService<ILoggerFactory>();
                    var logger = loggerFactory.CreateLogger<Program>();
                    var env = app.ApplicationServices.GetRequiredService<IWebHostEnvironment>();
                    var config = app.ApplicationServices.GetRequiredService<IConfiguration>();

                    logger.LogInformation("Logged in Configure");

                    if (env.IsDevelopment())
                    {
                        app.UseDeveloperExceptionPage();
                    }
                    else
                    {
                        app.UseExceptionHandler("/Home/Error");
                        app.UseHsts();
                    }

                    var configValue = config["MyConfigKey"];
                });
            });
        });
}
```

### Extend Startup with startup filters
This is another way of configuring middlewares the key difference is that instead of calling `app.Use{Middleware}` in the `Configure` method we inject a type implementing `IStartupFilter` which internall calls the `Use{Middleware}` method. as shown in the code below.

The Middleware
```
public class RequestSetOptionsMiddleware
{
    private readonly RequestDelegate _next;

    public RequestSetOptionsMiddleware( RequestDelegate next )
    {
        _next = next;
    }

    // Test with https://localhost:5001/Privacy/?option=Hello
    public async Task Invoke(HttpContext httpContext)
    {
        var option = httpContext.Request.Query["option"];

        if (!string.IsNullOrWhiteSpace(option))
        {
            httpContext.Items["option"] = WebUtility.HtmlEncode(option);
        }

        await _next(httpContext);
    }
}
```

The RequestSetOptionsMiddleware is configured in the RequestSetOptionsStartupFilter class:
```
public class RequestSetOptionsStartupFilter : IStartupFilter
{
    public Action<IApplicationBuilder> Configure(Action<IApplicationBuilder> next)
    {
        return builder =>
        {
            builder.UseMiddleware<RequestSetOptionsMiddleware>();
            next(builder);
        };
    }
}
```

The IStartupFilter is registered in the service container in ConfigureServices.
```
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
           .ConfigureAppConfiguration((hostingContext, config) =>
           {
           })
         .ConfigureWebHostDefaults(webBuilder =>
         {
             webBuilder.UseStartup<Startup>();
         })
        .ConfigureServices(services =>
        {
            services.AddTransient<IStartupFilter,
                      RequestSetOptionsStartupFilter>();
        });
}
```
