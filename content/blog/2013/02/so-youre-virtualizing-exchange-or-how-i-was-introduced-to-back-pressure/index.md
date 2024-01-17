---
title: "So you're Virtualizing Exchange or how I was introduced to Back pressure"
date: "2013-02-08"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "back-pressure"
  - "event-id-15007"
  - "exchange"
  - "sysadmin"
  - "technology"
---

The modern data centre or server farm looks very different from 5 years ago. It wasn't too long ago when each major role required not only another copy of your operating system (or an enterprise license) but another call to your favourite hardware vendor. Then you would be waiting eagerly for your shiny new bit of kit to arrived all covered in cardboard and bubble rap. When it was all said and done another server was drawing more power and sucking more precious aircon.

These days, it's all about Virtualization. Provisioning a new server is just a few clicks on your hypervisor of choice (usually something of the Microsoft, VMware or Citrix variety) and you are configuring server names and IPs not to mention adding roles. With the massive uptake in virtualization (of servers namely), comes new technology helping us maximise the resources provided by the host hardware. One of these technologies that aims to do just that is Dynamic Memory;

> Dynamic Memory is a new Hyper-V feature that helps you use physical memory more efficiently. With Dynamic Memory, Hyper-V treats memory as a shared resource that can be reallocated automatically among running virtual machines. Dynamic Memory adjusts the amount of memory available to a virtual machine, based on changes in memory demand and values that you specify.
> 
> [http://technet.microsoft.com/en-us/library/ff817651(v=ws.10).aspx](http://technet.microsoft.com/en-us/library/ff817651(v=ws.10).aspx)

In essence. Dynamic Memory increases and reduces the amount of RAM available to the guest Virtual Machine without interaction from the administrator.

## DO NOT USE DYNAMIC MEMORY WITH EXCHANGE

I hear you say _"But James, you just got me excited about Dynamic Memory and how my host hardware will be used in an efficient manner and Exchange is a massive memory garbage disposable machine that will eat any memory I throw at it"._

I hear you, one can sympathise; Dynamic Memory is one hell of a neat trick in Virtualization but if you use Dynamic Memory with your Exchange Virtual Machine (yes, even the [just released Exchange 2013](http://www.microsoft.com/exchange/en-us/exchange-overview.aspx "The New Exchange")) you will soon become familiar with the feature 'Back pressure'.

> Back pressure is a system resource monitoring feature of the Microsoft Exchange Transport service that exists on Microsoft Exchange Server 2010 Hub Transport and Edge Transport servers. Exchange transport can detect when vital resources, such as available hard disk space and memory, are under pressure, and take action in an attempt to prevent service unavailability.
> 
> [http://technet.microsoft.com/en-us/library/bb201658(v=exchg.141).aspx](http://technet.microsoft.com/en-us/library/bb201658(v=exchg.141).aspx)

In laments terms, Back pressure is feature introduced in Exchange 2007 that if your Exchange host became critically low on resources it required, rather than take everything down with it in a blaze of glory it would hold/deny incoming connections until the low resources situation was taken care of.

Quite a thoughtful feature don't you think?

Unfortunately, due to the way Back pressure works, and the nature of Dynamic Memory's efficiency they are not a match made in heaven.

The modern (2007 and up) Exchange is built with efficiency in mind. Constantly doing garbage collections and monitoring resource levels to keep itself in check. But how can it possible do its job when the playing field is constantly changing? It can't... Instead, Exchange think's its low on resources (in this case, memory) and throws an error such as **Event ID 15007**

> The Microsoft Exchange Transport service is rejecting message submissions because the service continues to consume more memory than the configured threshold.
> 
> Resource utilization of the following resources exceed the normal level: Private bytes = 75% \[High\] \[Normal=71% Medium=73% High=75%\]
> 
> Back pressure caused the following components to be disabled: Inbound mail submission from Hub Transport servers Inbound mail submission from the Internet Mail submission from the Pickup directory Mail submission from the Replay directory Mail submission from Mailbox servers Loading of e-mail from the queuing database (if available)
> 
> The following resources are in the normal state: Queue database and disk space ("c:\Program Files\Microsoft\Exchange Server\TransportRoles\data\Queue\mail.que") = 22% \[Normal\] \[Normal=94% Medium=96% High=98%\] Queue database logging disk space ("c:\Program Files\Microsoft\Exchange Server\TransportRoles\data\Queue\") = 22% \[Normal\] \[Normal=94% Medium=96% High=98%\] Version buckets = 0 \[Normal\] \[Normal=80 Medium=120 High=200\] Physical memory load = 57% \[limit is 94% before message dehydration occurs.\]

Sure, your Dynamic Memory will kick in and give it a boost of memory but in the mean time your mail will be delayed until Back pressure performs its next polling. This is unacceptable in this day and age of modern, quick, precise communication.

## Your options?

Stick to static memory configurations, _just like the old days._
