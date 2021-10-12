---
author: "Abhilash"
title: "ASP.NET Core Filters vs Middleware by example"
date: "2021-10-11"
description: "Explains the difference between asp.net core middleware and action filters with an example of adding a culture provider"
tags: ["asp.net core"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

## Whats a middleware ?
These are components in the ASP.NET Core request processing pipeline. Each such component is responsible to handle an incoming request execute some logic and pass on the control to subsequent component or shortcircuit the request. Essentially the request pipeline is built on these components.

## Whats a filter ?
Filters are components that run in the ASP.NET Core action invocation pipeline or simply the `filter` pipeline. The filter pipeline runs after ASP.NET Core selects an action to execute. So in a sense filters are extensions to the request processing pipeline which allow logic to be executed at different stages of the action handling.

 {{<figure src="images/filter-pipeline-1.png" >}}

### Types of filters
Since we can hook into before or after an action is executed they can be used to address a variety of cross cutting concerns at a single place. For instance, instead of writing duplicate authorization code for each action we could create an authorization filter to handle auth for each action. Based on their function filters can be broadly classified into following

#### Authorization Filters




## Middleware vs Filters

## An example for Middleware

## An example for Filter

## An example for Both & reusing a middleware in a filter

## References
https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters?view=aspnetcore-5.0#how-filters-work
https://andrewlock.net/exploring-middleware-as-mvc-filters-in-asp-net-core-1-1/
https://andrewlock.net/series/adding-a-url-culture-provider-using-middleware-as-filters/
https://stackoverflow.com/questions/42582758/asp-net-core-middleware-vs-filters
https://stackoverflow.com/questions/50887540/exceptionhandling-middleware-vs-filter-aspnetcore-webapi-2-1
https://www.edgesidesolutions.com/middleware-vs-filters-asp-net-core/
