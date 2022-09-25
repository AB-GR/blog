---
author: "Abhilash"
title: "OAuth and OpenId Connect"
date: "2022-09-25"
description: "Exploring OAuth for authorization and OpenId connect authentication"
tags: ["asp.net core", "oauth", "openIdconnect", "web app security"]
ShowToc: false
ShowBreadCrumbs: false
draft: true
---

### What is OAuth and OpenId Connect are they the same ?
Recently I was looking into an implementation of [openiddict](https://github.com/openiddict/openiddict-core) which as per the readme aims at providing a versatile solution to implement OpenID Connect client, server and token validation support in any ASP.NET Core 2.1 (and higher) application. Delving deeper I realized the need to understand OAuth and OpenId connect better as openiddict uses all the jargons and terminologies related to these specifications. I chanced upon very simple [explanation](https://www.youtube.com/watch?v=996OiexHze0) by Nate Barbettini and also a very good [series](https://dev.to/robinvanderknaap/setting-up-an-authorization-server-with-openiddict-part-i-introduction-4jid) by [Robin van der Knaap](https://dev.to/robinvanderknaap) for setting up authorization using openiddict.

### Traditional Authn & Authz
Usually in asp.net core we have out of the box mechanisms for Authentication and Authorization supported. As far as authentication is concerned [ASP.NET Core Identity](https://learn.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-6.0&tabs=visual-studio) is a complete, full-featured authentication provider for creating and maintaining logins. 
This is coupled along with an Authentication scheme such as [Cookie based Authentication scheme](https://learn.microsoft.com/en-us/aspnet/core/security/authentication/cookie?view=aspnetcore-6.0) which provides the mechanism for transfer of user identity data back and forth between the client and the server. However, a cookie-based authentication provider can be used without ASP.NET Core Identity.

Similary underlying authorization is claims based on top of which we have the declarative [role](https://learn.microsoft.com/en-us/aspnet/core/security/authorization/roles?view=aspnetcore-6.0) and a rich [policy-based](https://learn.microsoft.com/en-us/aspnet/core/security/authorization/policies?view=aspnetcore-6.0) model.

### Limitations of the custom Authn & Authz mechanisms
So what is wrong in doing authn and authz the traditional way ? Well there is not wrong if you are building a standalone web application and the users of the system dont need to collaborate with third party applications using their data on your system.

A simple case in point is third party login using google

### What is OAuth

### What is OpenIdConnect

### Are they the same ?

### Where does OpenIdConnect come in

https://chsakell.com/2019/03/11/asp-net-core-identity-series-oauth-2-0-openid-connect-identityserver/
