---
title: "The What"
date: "2024-01-23T22:44:13+11:00"
series: ["Wordpress to Hugo migration"]
series_order: 1
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "sysadmin"
  - "technology"
  - "hugo"
  - "wordpress"
  - "azure"
  - "static web app"
coverImage:
draft: true
---

As touched upon in my [recent post]({{< ref "blog/2024/01/were-back" >}}), this infernal machine has had a bit of a facelift. But the changes are more than pixel deep.

Behind the scenes, the blog has been migrated from the Wordpress content management system to Hugo, a static site generator.

Let's explore what these two systems are and are not to provide some context when we delve into why this migration took place.

## What is Wordpress?

![](images/wp_logo.png)

Wordpress as a system can be broken into two main components; **template files** written in PHP and a **database** (either MySQL or MariaDB). When combined together, they dynamically generate the content management system (CMS), which servers as the website's front end an author interacts with to create and display their content (otherwise known as a what you see is what you get (WYSIWYG) editor).

{{< mermaid >}}
flowchart LR
A("`Template
files`") & B[(Database)] --> C("`Content
Management
System`")
{{< /mermaid >}}

## What is Hugo?

![](images/hugo_logo.png)