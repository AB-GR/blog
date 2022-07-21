---
author: "Abhilash"
title: "Microservices with .NET"
date: "2022-07-21"
description: "The why, what & how of microservices in .NET"
tags: ["microservices"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

### Source Video(s)
https://www.youtube.com/watch?v=atJkRk_MwdU

### Why Microservices
One of the key reasons that microservices came about is to introduce agility into delivery of products/services essentially by breaking down a big problem(product) into smaller manageable pieces.

### What was the precursor to Microservices
The Monolith - Any product or a set services which acts as a single unit logically and physically(based on deployment) with a single data store, can be called as a Monolith. Now there are some problems associated with Monoliths.

#### Source code management
Usually Monoliths are a single solution with multiple projects, now this is a good way when we start the development, but soon becomes bloated as size of the product/service grows, leading to a great deal of delay in the development workflow. There are ways to segregate solutions in multiple ones but that usually only adds to the existing complexity.

#### Team management
Large projects typically have large teams working on them which increases the management/communication aspect of development work. And as the project scales this aspect usually suffers and the results could be slow decision making, error prone features etc.

#### Deployments are a nightmare
Over a period time, deployment cycles become larger as the cost of pushing features(development & testing) increases.

### How does Microservice help
Microservices are smaller single responsibitlity services, they have clear boundary and usually their own data store, for e.g a Monolith Ecommerce app can be broken down into multiple services such as product service, order service, inventory service, user service etc. The data and internal logic is hidden and can be accessd only through a contract exposed by the service

{{<figure src="images/whatism1.png" >}}

{{<figure src="images/whatism2.png" >}}

#### Microservices deployment
Usual way of deploying on a single app server would not be fault tolerant and switching to multiple app servers would be too costly, same with database using multiple of them would skyrocket the overall cost.

#### Containers to the rescue
{{<figure src="images/whatism3.png" >}}

### When to go for Microservices
Start with a Monolith if not sure, but think of moving to Microservices if agility of deployments and testing start ti become a concern and then about which parts can be independently tested and deployed.

### Tools Needed
Docker for packaging and deployment. When you start to hit 50-100 microservices you need to start thinking about an orchestration engine like Kubernetes.

### Microservice Communication
REST Api(external), Grpc(internal), Message based communication(queue, pub sub)

### How do i convert a Monolith to Microservice
Pluck the easily movable modules like authentication first, start isolating the table set i.e store in the same db but the data can be accessed only through API, also you can use cloud managed databases like DynamoDd in AWS & Cosmos Db in Azure.

### What about reports having data across databases
Either use gateway aggregators to aggregate data from queries across services or use a data stream to collect all the data changes and have a report microservice read from the same 

{{<figure src="images/whatism4.png" >}}

### Other important design considerations
Think about resilience in your design, decide when to use what kind of communication(REST, Messages, Grpc) etc. Logging and monitoring is essential for Microservices and should not be an afterthought(use a correlationId). Dont build a distributed monolith services communicate only through contracts.
