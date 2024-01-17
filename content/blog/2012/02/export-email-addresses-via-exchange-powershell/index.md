---
title: "Export email addresses via Exchange Powershell"
date: "2012-02-28"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "exchange"
  - "exchange-2007"
  - "exchange-2010"
  - "powershell"
  - "sysadmin"
  - "technology"
---

The other week I had a request for a list of all email addresses of staff for use with a legacy VOIP system. Seems simple enough right but in Exchange 2007 (possibly 2010 as well) there is no option via the GUI to pump out all the email addresses from your Exchange server.

Where there is a problem, we shall find a solution right?

After a bit of Google-fu & testing, I give you a script to output all SMTP addresses to an Excel document

## Requirements

- Exchange Powershell (tested 2007, should work on 2010)

## Steps

```powershell
Get-Mailbox -ResultSize Unlimited |Select-Object DisplayName,ServerName,PrimarySmtpAddress, @{Name=“EmailAddresses”;Expression={$_.EmailAddresses |Where-Object {$_.PrefixString -ceq “smtp”} | ForEach-Object {$_.SmtpAddress}}} | Export-Csv c:\mailbox_alias.csv
```

If we examine the code above you can see that this will export all SMTP address to a CSV formatted file named ```mailbox_alias.csv``` in the root of ```c:\``` (make sure you have the rights to write there or change the path to somewhere you do have rights). This CSV file will have columns in the following layout:

| DisplayName | ServerName | PrimarySMTPAddress | EmailAddresses |
|---|---|---|---|

The important thing to note is the final column ```EmailAddresses``` will provide any secondary/added SMTP address for those you use more than one email address.

In my specific case I needed to get email addresses from one specific mailbox database (staff and students are on seperate databases to make it easier to apply mailbox limits) and thus the script is a little different:

```powershell
Get-Mailbox -Database "Mailserver\Databasename" -ResultSize Unlimited |Select-Object DisplayName,ServerName,PrimarySmtpAddress, @{Name=“EmailAddresses”;Expression={$_.EmailAddresses |Where-Object {$_.PrefixString -ceq “smtp”} | ForEach-Object {$_.SmtpAddress}}} | Export-Csv c:\mailbox_alias.csv
```

You will note the addition of ```-Database "Mailserver\DatabaseName"``` to the script which allows you to choose a specific mailbox database to pull email addresses from.

So, there you have it; all your email addresses outputted to a nicely formatted CSV file. Too easy!
