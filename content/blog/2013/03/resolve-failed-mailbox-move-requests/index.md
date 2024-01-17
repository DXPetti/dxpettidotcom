---
title: "Resolve failed Mailbox Move Requests"
date: "2013-03-07"
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

So chances are, after reading my previous post on how to [move Mailboxes via Organizational Unit]({{< ref "blog/2013/02/mailbox-move-request-by-organizational-unit" >}}) you have migrated a couple hundred mailboxes. There is a good possibility that a small number of them have the status of **Failed** and that if you dig in to the log of the transfer you will come across the following error:

> Error: This mailbox exceeded the maximum number of corrupted items that were specified for this move request.

Whenever the Exchange 2010 Mailbox Replication Service comes across a Mailbox with a corrupted piece of data (whether than be an attachment, calendar item etc...) it automatically skips the mailbox and continues to plow through the rest in your list. This is by design and follows the mantra of _"better safe than sorry"_. Fear not, if you are happy to accept data loss (more of this later) we can get around this. Head to your Exchange Powershell and enter the following:

```powershell
Get-MoveRequest | where {$_.Status -eq "Failed"} | Set-MoveRequest -BadItemLimit 100 -AcceptLargeDataLoss
```

The above will get all the mailbox move requests that have failed and set the maximum amount of corrupted items to 100 before it skips the entire mailbox (as what was previously occurring). Note the ```-AcceptLargeDataLoss``` switch is needed if you set your ```-BadItemLimit``` switch to anything about 50. Now that we have told Exchange that we don't mind a bit of data loss lets resume the move requests:

```powershell
Get-MoveRequest | where {$_.Status -eq "Failed"} | Resume-MoveRequest
```

Depending on the amount of corrupt items in each mailbox, with any luck they will be now on their way to your new Exchange server. If you get a couple that still don't move across and failed, just up the ```-BadItemLimit``` and try again. 

So I mentioned earlier that I would touch on the data loss aspect of the mailbox move and why it's not such a terrible thing to occur. From time to time, with the amount of calendar appointments, shared tasks and what not flying across several mailboxes at a time there comes a time that corruption can occur. This corruption can lead to unsavoury experiences for the end user, and this is not a good thing (you'll have to hear about it the next time you venture close to the water cooler). During this mailbox move, (when instructed) corrupted items are dropped from the mailbox and the end result is a pristine and clean mailbox, free from evil corruption. 

Microsoft even make use of this function to cleanse mailboxes that are apart of their Exchange Online cloud offering as detailed here: [Corrupted Items and Mailbox Moves in Exchange 2010.](http://www.windowsitpro.com/content1/topic/corrupted-items-mailbox-moves-exchange-2010-142659/catpath/exchange-server-2010/page/2  "Corrupted Items and Mailbox Moves in Exchange 2010") If you have multiple Exchange 2010/13 Mailbox servers it might be wise to shuffle the mailboxes between them every now and again to perform a cleansing ritual...that or overkill.
