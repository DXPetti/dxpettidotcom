---
title: "Bulk Backup and Set Out of Office Messages"
date: "2020-04-06"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "exchange"
  - "exchange-2010"
  - "powershell"
  - "sysadmin"
  - "technology"
coverImage: "ooobackupset_cover.jpg"
---

We live in very interesting times at the moment. No doubt, anyone reading this has been touched by the effects of the spread of COVID-19 across the world.

One requirement that I had in response to the outbreak was to prepare for a full office shutdown. Part of this shutdown would require all of the businesses shared/group mailboxes to display a new Out of Office message to all external parties.

While this task in itself, isn't too complicated to appear, if the time came to do so, I wanted to ensure that any existing Out of Office message and corresponding configuration was preserved. This way, upon business operations returning to their former state, the previous Out of Office message/configuration can be restored. In doing so, we are taking the burden of the customer to do this (I'm sure they have enough on their plate to catch up on).

So, without further ado, bring forth the Powershell:

{{< gist DXPetti 287bd65dd794b67f7a734fabfdf0a223 >}}

Let's break down a couple of important elements to the code.

First and foremost, my code is targeted at a Exchange 2010 (yes I know, don't @ me) environment, therefore, I'm loading the Exchange 2010 snap-in:

```powershell
#Load Exchange Powershell Module
if (! (Get-PSSnapin Microsoft.Exchange.Management.PowerShell.E2010 -ErrorAction:SilentlyContinue) )
{
    Add-PSSnapin Microsoft.Exchange.Management.PowerShell.E2010 -ErrorAction:Stop
}
```

Feel free to swap out with your version of Exchange that you will be targeting. I do warn you that YMMV. I've had random crashes loading or utilising the Exchange modules outside of the Exchange Shell. My recommendation would be to use this code with the Exchange shell versus a regular Powershell shell.

Next up, we need to get all mailboxes of the shared/group variety. Fortunately, the environment I am working on has keep these types to a specific OU, which I am targeting with:

```powershell
#Get all Shared Mailboxes and loop through each mailbox
$Mbx = Get-Mailbox -OrganizationalUnit "OU=Shared Mailboxes,OU=Users,DC=contoso,DC=co"
```

Replace the OU with your equivalent. The alternative is to make use of ```Get-Mailbox```'s -[RecipientTypeDetails](https://docs.microsoft.com/en-us/powershell/module/exchange/mailboxes/get-mailbox?view=exchange-ps#parameters) parameter to specify the mailbox type, if you are making use of them in your environment.

Apart from the above two change you may need to make, all you need to do is run the cmdlet and provide both the path for the backup of current OOO message + configuration to be stored plus the new message and your set to go.

So backing up and setting a message in bulk is now straight forward to achieve in your organisation. How do we rollback our OOOs once things settle down and return to normal?

Look out for my [next post]({{< ref "blog/2020/05/restore-out-of-office-backup-with-powershell" >}}) to learn how.

Stay safe, stay indoors!
