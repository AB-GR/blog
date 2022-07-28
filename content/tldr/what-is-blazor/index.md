---
author: "Abhilash"
title: "What is blazor"
date: "2022-07-27"
description: "Brief description of blazor and the tech behind it"
tags: ["blazor", "spa"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

### Source Article(s)
https://docs.microsoft.com/en-us/aspnet/core/blazor/?view=aspnetcore-6.0
https://blog.stevensanderson.com/2018/02/06/blazor-intro/
https://blazor-university.com/overview/what-is-blazor/
https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/choose-between-traditional-web-and-single-page-apps


### What is Blazor
Blazor is a framework to build interactive client-side web UI using .NET & C# instead of Javascript.

### Advantages of Blazor
 - Depend only on C# & its runtime which is time tested and feature rich to build apps end to end.
 - Share app logic across client and server.
 - Render UI as html for browser wide support including mobile browsers.
 - Create hybrid apps for desktop and mobile.

### But how does it work ?
Blazor provides 3 technical approaches that help run C# code on a browser.

#### 1. Blazor WebAssembly(wasm)
In this approach blazor uses WebAssembly(wasm), which is a bytecode instruction set based on open web standards which is designed to run on any host(mainstream browsers) which is capable of interpreting those instructions, or compiling them to native machine code and executing them.

.NET which usually compiles languages into bytecode therefore considers Webassembly(wasm) as yet another target platform. Therefore Blazor runs on webassembly in two modes a) Interpreted b) AOT compiled

#### Interpreted mode
In this mode the .NET Runtime itself is compiled into webassembly the browser then loads the runtime, which in turn loads and executes the application assemblies in an interpreted manner.

{{<figure src="images/blazor-1.png" height="350" width="350" >}}

#### Ahead-of-time (AOT) compiled mode
In AOT mode, your application’s .NET assemblies are transformed to pure WebAssembly binaries at build time. At runtime, there’s no interpretation: your code just executes directly as regular WebAssembly code. It’s still necessary to load part of the Mono runtime (e.g., parts for low-level .NET services like garbage collection), but not all of it (e.g., parts for parsing .NET assemblies).

{{<figure src="images/blazor-2.png" height="350" width="350">}}

#### 2. Blazor Server
In this model, the Blazor application runs on the server on top of the full .NET Core runtime. When the user loads the application a small JavaScript file is downloaded which establishes a real-time, two-way SignalR connection with the server. Any interaction that the user has with the app is then transmitted back to the server over the SignalR connection for the server to process. After the server is done, any UI updates are transmitted back to the client and applied to the DOM.

{{<figure src="images/blazor-3.png" height="300" width="400" >}}

#### 3. Blazor Hybrid
Hybrid apps use a blend of native and web technologies. A Blazor Hybrid app uses Blazor in a native client app. Razor components run natively in the .NET process and render web UI to an embedded Web View control using a local interop channel. WebAssembly isn't used in Hybrid apps. Hybrid apps encompass the following technologies: .NET MAUI, WPF, Windows Forms etc.

### Need to get more done on the browser ? enter JavaScript interop
For apps that require third-party JavaScript libraries and access to browser APIs, components interoperate with JavaScript. Components are capable of using any library or API that JavaScript is able to use. C# code can call into JavaScript code, and JavaScript code can call into C# code.

### When to choose Blazor
 - Your application must expose a rich user interface
 - Your team is more comfortable with .NET development than JavaScript or TypeScript development