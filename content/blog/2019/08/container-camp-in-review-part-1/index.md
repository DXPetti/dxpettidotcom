---
title: "Container Camp in review - Part 1"
date: "2019-08-05"
series: ["Container Camp"]
series_order: 1
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "containers"
  - "docker"
  - "k8s"
  - "kubernetes"
  - "linux"
  - "sysadmin"
  - "technology"
coverImage: "ccau2019_cover1.jpg"
---

For those of you who follow me on other social media platforms, it will be of no surprise that I had the fortunate opportunity to attend Container Camp Australia this year.

<iframe src="https://www.linkedin.com/embed/feed/update/urn:li:share:6556360128902377472" height="230" width="504" frameborder="0" allowfullscreen title="Embedded post"></iframe>

Previously, I never the pleasure to experience a technical conference in my professional career. At most, I've been to meetups and user group events such as [VMUG]({{< ref "blog/2014/02/vmware-vmugs" >}}). Safe to say, I was pretty pumped.

{{< tweet dxpetti 1153835703605686272 >}}

Enough of that though, the real question is, what is Container Camp?

## What is Container Camp?

Do you eat, breathe and sleep [Docker]({{< ref "/tags/docker" >}})? Are you looking to abstract away your infrastructure so your developers can just focus on their application code? Like to hear war stories from the front lines of digital transformation using technologies such as [Kubernetes]({{< ref "/tags/kubernetes" >}})? Or are you absolutely bamboozled by the endless technical buzzwords and jargon in the brave new world of DevOps and SRE?

This is the 2 day technical conference for you!

Over the 2 days, the wide and varying levels of talks, discussions and demonstrations afforded me to absorb a considerable amount of information, not just about Containers but Developer and Operations workflows in use by modern workplaces (and how some archaic organisations transformed themselves into modern workplaces). I've tried to pick out, in no particular order, a couple of key takeaways that resonated with me during the conference.

## Always use User Namespaces

{{< tweet arjenschwarz 1154629405248876544 >}}

As part of Aleksa ([@lordcyphar](https://twitter.com/lordcyphar)) from SUSE Australia's talk that focused on securing container runtimes (and who better than a maintainer of [runc](https://github.com/opencontainers/runc) and the [OCI specifications](https://www.opencontainers.org/)).

Namespaces in the Linux world are partitions or logical fences around resources on a host. These resources range from access on the file system, time on the CPU, area of Memory etc... The are used to put barriers in place to prevent processes running away with your host and potentially taking it over.

User Namespaces focus on _(you guessed it...)_ user partitions by mapping a user for a particular process, or in the case of this topic, a container to a user of no or less privilege on the host. This is particular important to mitigate privilege escalation attacks where by the an attacker has gained access to the root user (potentially your application within the container requires it). Once free from the container bounds, because we have mapped the user to one of less or no privilege on the host, they are dead in their tracks.

One particular humorous anecdote for me personally in this talk was when Aleksa projected this for everyone to see:

![](images/ccau20191.jpg "slide source: [cyphar's github](https://github.com/cyphar/talks/blob/71ab9be9919797c2624dfc17037d6c083bc00985/2019/07-containercamp/securing-container-runtimes.odp)")

As I came to understand and become more familiar with Linux. One of the greatest pleasures of working with Linux versus my Windows daily driving is everything is represented as a file. No objects, registry etc... just plain, good ol, text file. Apparently, this is also very bad for security, specifically, how quickly an attack can spread it seems...Look at all those CVEs!

In the next three posts, we are going to dive into Service Meshes, their reason for being and which one is for you.
