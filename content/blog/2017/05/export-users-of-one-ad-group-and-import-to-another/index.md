---
title: "Export users of one AD group and import to another"
date: "2017-05-11"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "active-directory"
  - "powershell"
  - "sysadmin"
  - "technology"
  - "windows"
---

It's quite common for members of one Active Directory security group to be replicate in another. Whether it is for testing linked systems (like System Center Configuration Manager linked collections) or simply all the members that have access to a location on the network need access to another.

Despite how common a action it is, it is rather painful to do in Active Directory Users and Computers snap due to the inability to copy/paste members of a group.

Never fear, let us lean on our good friend PowerShell to get the job done. We can combine the ```Get-ADGroup``` and ```Add-ADPrincipalGroupMembership``` commands to do what we need like below:

```powershell
Get-ADGroup -Identity "group1" | Add-ADPrincipalGroupMembership -MemberOf "group2"
```

Simply replace _group1_ and _group2_ values with the name of the source and destination groups respectively. We utilise ```Add-ADPrincipalGroupMembership``` due to a very similar command, ```Add-ADGroupMember```; cannot receive objects through a pipeline.

Similarly, we can remove all members of one group from another like so:

```powershell
Get-ADGroup -Identity "group1" | Remove-ADPrincipalGroupMembership -MemberOf "group2"
```

Once again, replace _group1_ and _group2_ values, this time with the name of the source group who's members you want to remove from destination group respectively.

PoSH on dudes!
