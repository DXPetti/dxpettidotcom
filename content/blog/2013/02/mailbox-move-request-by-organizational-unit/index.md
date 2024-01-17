---
title: "Mailbox Move Request by Organizational Unit"
date: "2013-02-21"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "exchange"
  - "exchange-2010"
  - "powershell"
  - "sysadmin"
  - "technology"
---

I'll admit, I do love Active Directory. It allows me create great structure and sound groupings of a otherwise chaotic bunch of silicone and digital bits and bytes. As much as I love it, unfortunately not all applications can take advantage of a well organized Active Directory, especially in a GUI environment.

Exchange 2010 is one of these applications.

When performing Mailbox Move Requests in the GUI Exchange Management Console you are restricted by the search filters baked in. While this can serve most people and for most day to day functions; if you are doing things in bulk, sometimes you need a bit more control. For instance, what if I want to migrate mailboxes by Organizational Unit? Maybe I want to move the folks over in Accounting first, because they are known trailblazers (HA!). There is no option in the Management Console to define your search filter by Organizational Unit (many of the filters that can be utilized are purely Exchange attributes, which is puzzling because of the tight relationship AD and Exchange have).

Enter Exchange Powershell.

```powershell
Get-Mailbox -OrganizationalUnit "OU=Accounting,OU=Users,DC=Contoso,DC=com" | New-MoveRequest -TargetDatabase "Internal Staff"
```

Nice and simple.

We have used ```Get-Mailbox``` to retrieve all the mailboxes from the specified Organization Unit (which must be in LDAP path format) and then piped it into the ```New-MoveRequest``` query and targeted a specific Mailbox Database.

When the GUI lets you down, turn to Powershell folks.

Happy Friday
