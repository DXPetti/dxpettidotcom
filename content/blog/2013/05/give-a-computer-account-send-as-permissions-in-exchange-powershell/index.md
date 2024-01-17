---
title: "Give a computer account Send-As permissions in Exchange Powershell"
date: "2013-05-06"
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

Take for instance the File Server Resource Manager that is a feature of the File Server role. It has the ability to send out emails to end users and administrators alike, warning them over quota usage. Unfortunately, there is no option to set up authentication methods in the application, instead the computer account is used to send the email to the email server.

Once again, the Exchange Management Console falls down in this respect; yes you can manage Send-As permissions for mailbox accounts but only if the account you wish to give permissions to is a user or group account. History tells us when the GUI comes up short, Powershell gets the job done; this time is no different.

To set a computer account to have send-as permission for a specific mailbox input the following into an Exchange Powershell window:

```powershell
Add-ADPermission -Identity "mailboxname" -User "DOMAIN\computername$" -ExtendedRights "Send-as"
```

Replace ```mailboxname``` with the name of the mailbox requiring the permissions and replace ```DOMAIN\computername$``` with the name of your domain (NETBIOS is fine) and the name of the computer that will be sending the emails.

Powershell solving another one of sysadmin's issues!
