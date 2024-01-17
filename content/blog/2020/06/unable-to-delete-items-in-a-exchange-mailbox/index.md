---
title: "Unable to delete items in a Exchange Mailbox"
date: "2020-06-08"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "exchange"
  - "exchange-2010"
  - "outlook"
  - "powershell"
  - "sysadmin"
  - "technology"
coverImage: "recoverableitems_cover.jpg"
---

Have you ever got yourself into a situation in Outlook or OWA whereby you are cleaning out a mailbox and find you can no longer delete anymore items from the mailbox?

I had the pleasure of coming across such a mailbox recently. It was piling up items in the Junk Email folder like someone stockpiling gold in a end of days financial event. Corrupted items or mailbox is were initial thinking went but even after a mailbox move, the thousands of junk email items were still there and still unable to be deleted.

It never occurred to me that when items are deleted, followed by transferring to the Recoverable Items special folder, that when this folder hit's its limit, further deletions are not possible. But this is exactly what has occurred.

So what is the limit for recoverable items? Let's grab the quota on a mailbox of your choosing with Exchange Powershell:

```powershell
Get-Mailbox "John Anderson" | Format-List *Quota*,RetainDeletedItemsFor,UseDatabaseRetentionDefaults
```

This will help us glean key quota limits on a particular mailbox as well as tell us how long deleted items are held in Recoverable Items folder for. In this instance, we are focused on the ```RecoverableItemsQuota``` limit.

Once we understand the limit, let's verify that we have approached the limit with:

```powershell
Get-MailboxFolderStatistics "John Anderson" -FolderScope RecoverableItems
```

If you are in the same predicament, the size of your Recoverable Items folder should be close to or bouncing of the limit we listed earlier.

So we know the how much storage the Recoverable Items folder has and that we are using it all currently, how can we fix it? Fortunately, Exchange Powershell makes it rather trivial to purge just these items, no fancy filtering required:

```powershell
Search-Mailbox -Identity "John Anderson" -SearchDumpsterOnly -DeleteContent
```

Easy as that, Recoverable Items will be instantly purged and you can continue on with deleting items. However, if you aren't the cowboy type and need to ensure those items are stored somewhere for archival or analysis reasons, we can transfer them at the same time with the following:

```powershell
Search-Mailbox -Identity "John Anderson" -SearchDumpsterOnly -TargetMailbox "Internal Investigations -TargetFolder "John Anderson" -DeleteContent
```

Now our Investigations team has a copy of all the items that were in John Anderson's bin while John can continue on deleting more of the junk that is piling up in his mailbox.
