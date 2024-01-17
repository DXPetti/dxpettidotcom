---
title: "Search DHCP for a MAC Address with Powershell"
date: "2018-09-10"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "dhcp"
  - "powershell"
  - "sysadmin"
  - "technology"
coverImage: "dhcp_posh_cover.jpg"
---

[Many many moons ago]({{< ref "blog/2015/06/searching-windows-server-dhcp-via-mac-address" >}}), we had a way to trawl through DHCP scopes utilising the netsh tool to find devices on our networks that match a certain MAC address.

Instead of relying on such an outdated tool that isn't on Microsoft's radar to keep alive, I've cobbled together a function in Powershell aptly named ```Get-Mac``` to perform some duty which added functionality (and no required to log onto DHCP hosts).

{{< gist dxpetti dc2ac3fa4785e765deeec20e8b8e6c28 >}}

The above function will by default, utilise the ```Get-DhcpServerInDC``` to return all your DHCP servers, obtain all their scopes with ```Get-DhcpServerv4Scope```, look through each scopes lease with ```Get-DhcpServerv4Lease``` and finally match any lease with the MAC address provided. If you have a large enterprise network, comprises of multiple AD sites, then you can narrow the field down with the ```-DhcpSite``` together with the DNS name of your DHCP server in that site. Both the ```-Mac``` and the ```-DhcpSite``` switches work on wildcard matches so you won't need the full address or full name of the DHCP server. Hopefully your AD sites have a naming convention which should assist narrowing it down.

Let me know if you found this one useful in your SysAdmin'ing day or think of improvements.
