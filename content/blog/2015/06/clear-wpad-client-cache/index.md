---
title: "Clear WPAD client cache"
date: "2015-06-29"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "internet-explorer"
  - "proxy"
  - "sysadmin"
  - "technology"
  - "windows"
  - "wpad"
showTableOfContents: false
---

> # wpad
> ### noun \[no plural\]
>
> A wonderful but horrible way to automate clients connecting to the internet via proxy server.
>
> 'No I haven't configured proxy settings in IE, that's why we have the WPAD configured'

I remember a time before having implemented a WPAD file where by the proxy server was manually specified using IE Group Policy objects. This caused all kind of hell for customers who were mobile and used their device outside the bounds of the enterprise. So while it has it's horrible moments (that I'll go on to in a minute), it certainly can reduce a tonne of tickets coming a SysAdmin's way.

Once implemented though, there are times (rare, but they occur) when a client hangs on to a outdated version of your WPAD file. This can be show-stopping, especially if your WPAD file is used for white/black listing. To make things worse, there isn't a whole lot of visibility about what clients have what version.

Having said that, if you do have a customer who's machine is hanging on to a old WPAD file. do the following to clear the cache:

1. Via Internet Options, Advanced tab; reset Internet Explorers including personal files such as cookies etc...
2. Ensure all instances of Internet Explorer (and any other browser) are closed, open up a Administrative Command Prompt and input the following:
   ```cmd
   del \wpad*.dat /s
   ```
   This will hunt down all instances of the WPAD across your disk, list and remove them.  

3. While still at the Administrative Command Prompt, enter:
    
   ```cmd
   ipconfig /flushdns
   nbtstat -R
   ```
   {{< alert "circle-info" >}}
   That last command **is case sensitive**.
   {{< /alert >}}

   This will flush your DNS cache as well as NetBIOS cache.  

Once you fire up Internet Explorer or other browser, the machine will now grab a fresh copy of the WPAD file. Horrors of the automatic proxy configuration file begone!
