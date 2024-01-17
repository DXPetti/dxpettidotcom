---
title: "Cleaning up corrupt Registry.pol files with Powershell"
date: "2018-10-22"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "group-policy"
  - "powershell"
  - "registry"
  - "sysadmin"
  - "technology"
  - "windows-10"
  - "windows-7"
coverImage: "regpol_cover.jpg"
---

One ongoing issue that can occur across an predominately Windows/Group Policy heavy enterprise environment is the corruption of the Registry.pol file located in _%windir%\system32\Group Policy\Machine\\_. This file contains all the machine-based Group Policy settings in Registry format and are loaded at Operating System startup.

For reasons not even known by Microsoft it seems, this file can occasionally get corrupt and centrally defined Group Policies are no longer updated/kept in sync.

So in a effort to be able to clean up such a corruption at scale, I created a Powershell script that:

1. Takes a array of machines as input
2. Confirms they are reachable over the network
3. Confirms that it has permissions to the location of Registry.pol
4. Check the Date Modified tag of the file and if older than 1 day (good sign of corruption), delete the file and force a Group Policy refresh

One of the caveats to this process was while there a many more cmdlets in Powershell V3+ that I could leverage, it still had to support Windows 7 machines at the time of writing and therefore leverage much less cleaner ways to kick of a Group Policy refresh.

Without further ado...

{{< gist dxpetti f5f81035ebe77c76de3050532b21f184 >}}

Would love to see what improvements the readers out there could make. Maybe make use of job batching to do it in a much more parallel fashion? Let me know if you do give it a try in your environment.
