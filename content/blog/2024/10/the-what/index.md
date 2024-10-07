---
title: "The What"
date: "2024-10-07T23:07:13+11:00"
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
coverImage: "thewhat_cover.jpg"
---

As touched upon in my [recent post]({{< ref "blog/2024/01/were-back" >}}), this infernal machine has had a bit of a facelift. But the changes are more than pixel deep.

Behind the scenes, the blog has been migrated from the Wordpress content management system to Hugo, a static site generator.

Let's explore what these two systems are and are not to provide some context when we delve into why this migration took place.

## What is Wordpress?

![](images/wp_logo.png)

Wordpress as a system can be broken into two main components; **template files** written in PHP and a **database** (either MySQL or MariaDB). When combined together, they dynamically generate the content management system (CMS), which servers as the website's front end an author interacts with to create and display their content (otherwise known as a what you see is what you get (WYSIWYG) editor).

{{< mermaid >}}
flowchart LR
T("`Template
files`") --> C("`Content
Management
System`")
D[(Database)] <--> C
{{< /mermaid >}}

So what is the workflow for someone using Wordpress? Regardless of if the above is self-hosted or using a Wordpress as a Service solution, the content author uses the same process.

1. The author will open a internet browser and point to the Wordpress instance
2. Authenticate against the instance
3. Use the WYSIWYG editor to create and then publish the content (either instantly or schedule for a future time/date)

{{< mermaid >}}
flowchart TD
B("Browser") -- authenticate --> W("Wordpress")
subgraph W [Wordpress]
C("Create content")
C --> P("Publish content")
C --> S("Schedule content") -- schedule met --> P
end
{{< /mermaid >}}

As one could surmise, Wordpress keeps things very simple and easy for authors. One of the primarly reasons for it's ubiquitousness. 

## What is Hugo?

![](images/hugo_logo.png)

Hugo as a system has one main component; a static site generator. This generator takes theming files and your content, written in [Markdown](https://commonmark.org/help/) and either renders locally or bundles up and generates the website in full, as static HTML. This bundle can then be viewed locally or used with virtually any kind of hosting platform, be it self-hosted or a service.

{{< mermaid >}}
flowchart LR
C("Content inc
themes") --> S("Static site
generator")
{{< /mermaid >}}

Just as Hugo is quite different to Wordpress in system architecture, it is quite different in workflow.

1. The author will open a text editor of their choice and create the content
2. Content is bundled with the theming files and either:
    1. Ran through Hugo locally where the entire site is generated as static HTML files. This is then published to a standard web host via traditional methods like FTP or rsync
    2. Uploaded to version control system like GitHub and then generated with Hugo via continous deployment to a linked service to publish such as Netlify, AWS Amplify or Azure Static Web Apps

{{< mermaid >}}
flowchart TD
T("Text editor") --> C("Create content")
C --> PL("Publish locally")
C -- version control--> PR("Publish remotely")
subgraph H [Hugo]
PL -- ftp rsync--> PR
end
{{< /mermaid >}}

Hugo, as you can see, decouples the creation of content from the publishing of content. This means you can use whatever toolset you are most comfortable with rather than be constrained by the vendor.

In part two, we'll explore the context of why I made the move from Wordpress to Hugo and understand why it might make sense for you to do so too.