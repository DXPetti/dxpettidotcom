---
title: "Container Camp in review - Part 2"
date: "2019-08-19"
series: ["Container Camp"]
series_order: 2
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "containers"
  - "devops"
  - "docker"
  - "faas"
  - "k8s"
  - "kubernetes"
  - "service-mesh"
  - "sre"
  - "sysadmin"
  - "technology"
coverImage: "ccau2019_cover2.jpg"
---

[Previously]({{< ref "blog/2019/08/container-camp-in-review-part-1" >}}) we touched upon the best practice of implementing user namespaces in your container ecosystem and how they are a pivotal element in the layers of security you can employ to protect your infrastructure/platforms.

Now let's dive into the glue that helps bind and make sense of your services once you are drowning in containers; Service Meshes!

## Service Mesh....Service what?

Before we break apart what exactly a Service Mesh is, we should first understand why we want, nay, need one!

The hottest word at the intersection of business land and the technical sphere (besides Kubernetes) is _Observability_. If you haven't heard of this term before, go and Google it. I'll wait right here...

In a watered down, layman terms explanation; Observability refers to the intersection of monitoring and testing. Observability not about tools. Observability is about insight, metrics and an understanding of the services you are providing to customers. While monitoring tools can be used to build your overall picture of observability, traditional monitoring of event driven logging is more or less focused on predictable failure scenarios (i.e. hard disk is full, network adapter is disconnected). Monitoring won't allow you in a pinch to answer the CIO/CTOs question of "why is this service running slowly" or "lots of people are adding items to their cart, but not proceeding with the purchase". The practice of observability tries to answer instead.

## Why observability?

So why the sudden surge of the desire for observability you ask? Culture change in the IT landscape, with moves to DevOps methodologies and Site Reliability (SRE) practices have forced a lot of people to step away from the confines of their favourite IDE, shell or dashboard and pivot their focus on business outcomes. At the forefront of achieving better outcomes are Containers and micro services. These constructs allow for much more rapid iteration and thus, business agility.

When organisations take a gigantic, archaic, monolithic application and break it down per business logic, divide it into containers or even server-less functions to consume via Docker/Kubernetes or a function service like [OpenFaaS](https://docs.openfaas.com/), [AWS Lambada](https://aws.amazon.com/lambda/), [Azure Functions](https://azure.microsoft.com/en-au/services/functions/) etc... the topology of this mass of interconnected spiderweb can be a daunting image:

![](images/ccau20192.jpg "what have we done!")

Sure, you've achieved the isolation that modern world security demands; each of your business functions can now be maintained, patched and updated in rapid succession without disrupting other functions but...that's a nightmare. Just look at it!

If you were asked about performance, whether it be network, storage, compute; where would you start? Your developers are just focused on writing good code, they don't want to inject monitoring of infrastructure in each container or function. How do you go about getting the necessary information to make informed decisions?

This is where our Service Meshes shine. With that in mind, let's explore what Service Meshes exactly are, next!
