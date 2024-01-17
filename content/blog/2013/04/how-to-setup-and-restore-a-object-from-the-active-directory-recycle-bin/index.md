---
title: "How to setup and restore a object from the Active Directory Recycle Bin"
date: "2013-04-15"
series: ["Active Directory Recycle Bin"]
series_order: 1
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "active-directory"
  - "powershell"
  - "sysadmin"
  - "technology"
---

CTRL+Z, the undo button, the recycle bin, shadow copies; The human element in the world of IT can some times be our undoing; This goes along way to explain the push to automate EVERY facet of the our IT systems. While some organisations are not big enough to justify automating to this degree or haven't yet invested the time to do so things like the Active Directory Recycle Bin are a nice stop-gap. The setup of Active Directory Recycle Bin is fairly straight forward:

1. All Domain Controllers must be running Windows Server 2008 R2 or higher (prep your Forest and Domain if you have not already)
2. Forest functional level must be at Windows Server 2008 R2 or higher
3. Run the following Powershell command:

```powershell
Enable-ADOptionalFeature –Identity ‘CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=yourdomain,DC=com’ –Scope ForestOrConfigurationSet –Target ‘yourdomain.com’
```

Now that the Recycle Bin has been enabled how do we go about restoring an object that has been deleted (obviously AFTER the Recycle Bin has been enabled ^_-)? Unfortunately there is no option to restore an object via the Active Directory Users and Computers snap-in as deleted objects are very much hidden no matter what options you make visible. Instead we must once again flex out Powershell skills with the following:

```powershell
Get-ADObject -Filter {DisplayName -eq "nameofobject" -and deleted -eq $true} -Include DeletedObjects | Restore-ADObject
```

Real simple this one, the ```Get-ADObject``` followed by some filtering and the ```-Include DeletedObjects``` switch gets our deleted object (just replace ```nameofobject``` with the display name of your Active Directory object that has been deleted). This is then piped to ```Restore-ADObject``` to restore the object in place where it was located when it was deleted.

Too easy right? What if you want to restore a whole tree of deleted objects (i.e. a delete Organisational Unit and all the objects inside)? I'll be tackling that in another post so stay tuned!
