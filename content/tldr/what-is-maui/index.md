---
author: "Abhilash"
title: "What is MAUI"
date: "2022-07-22"
description: ".NET MAUI in a nutshell"
tags: ["microservices"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

.NET Multi-platform App UI (.NET MAUI) is a cross-platform framework for creating native mobile and desktop apps with C# and XAML.
While .NET Multi-platform App UI (.NET MAUI) has been released, Visual Studio support for .NET MAUI is currently in preview

.NET MAUI is open-source and is the evolution of Xamarin.Forms, extended from mobile to desktop scenarios, with UI controls rebuilt from the ground up for performance and extensibility.

If you've previously used Xamarin.Forms to build cross-platform user interfaces, you'll notice many similarities with .NET MAUI. However, there are also some differences. Using .NET MAUI, you can create multi-platform apps using a single project, but you can add platform-specific source code and resources if necessary. One of the key aims of .NET MAUI is to enable you to implement as much of your app logic and UI layout as possible in a single code-base.


### How .NET MAUI Works
.NET 6 provides a series of platform-specific frameworks for creating apps: .NET for Android, .NET for iOS, .NET for macOS, and Windows UI 3 (WinUI 3) library. These frameworks all have access to the same .NET 6 Base Class Library (BCL). The BCL depends on the .NET runtime to provide the execution environment for your code. For Android, iOS, and macOS, the environment is implemented by Mono, an implementation of the .NET runtime. On Windows, .NET CoreCLR provides the execution environment.

{{<figure src="images/maui-arch1.png" >}}

In a .NET MAUI app, you write code that primarily interacts with the .NET MAUI API (1). .NET MAUI then directly consumes the native platform APIs (3). In addition, app code may directly exercise platform APIs (2), if required.