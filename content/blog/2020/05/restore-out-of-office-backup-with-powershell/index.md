---
title: "Restore Out of Office Backup with Powershell"
date: "2020-05-04"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "exchange"
  - "exchange-2010"
  - "powershell"
  - "sysadmin"
  - "technology"
coverImage: "ooorestore_cover.jpg"
---

In my [previous post]({{< ref "blog/2020/04/bulk-backup-and-set-out-of-office-messages" >}}), I discussed the need to set a special external facing Out of Office message for all shared mailboxes in the organisation. Prior to doing so, I ensured we backed up the current message, along with it's configuration.

Hopefully you followed along and did the same as now is the time to restore our old message and configuration.

Once again, we will look to the power of Powershell (heh) to help us out:

{{< gist DXPetti 2fce15e66034be8c8fc3802077944584 >}}

The restore cmdlet takes much of it's DNA from the backup cmdlet. Therefore the same rules apply.

Change the Exchange module to one that is applicable in your environment:

```powershell
#Load Exchange Powershell Module
if (! (Get-PSSnapin Microsoft.Exchange.Management.PowerShell.E2010 -ErrorAction:SilentlyContinue) )
{
    Add-PSSnapin Microsoft.Exchange.Management.PowerShell.E2010 -ErrorAction:Stop
}
```

And point the cmdlet to where your shared mailboxes are:

```powershell
#Get all Shared Mailboxes and loop through each mailbox
$Mbx = Get-Mailbox -OrganizationalUnit "OU=Shared Mailboxes,OU=Users,DC=contoso,DC=co"
```

Once the above two elements are matched to your environment, run the cmdlet. The only parameter you will need to provide is the path to where the backups are located.

![](images/simples.png "Simples")

You've now saved your customers a enormous amount of time not worrying about settings things back to how they were. Productivity is on the rise!
