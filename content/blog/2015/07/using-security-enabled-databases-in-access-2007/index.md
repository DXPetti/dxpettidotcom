---
title: "Using Security Enabled Databases in Access 2007+"
date: "2015-07-06"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "access"
  - "microsoft-office"
  - "sysadmin"
  - "technology"
---

There are some people out there, who are only just migrating away from very old systems **\*cough\*** Windows XP **\*cough\***. With these upgrades to a more modern operating system, comes upgrades to office productivity suites. There are many _gotchas_ when embarking on this path, one of which is using security enabled databases that came from Microsoft Office Access 2003 with a later version of Access.

To paint the scene quickly; with security enabled databases you have two files:

- .mdb file - this is your database containing your, erm... data
- .mdw file - this is the workgroup security information file containing access control lists

In previous versions of Access, simply double-clicking the .mdb file is enough to open the database, whereby you will be prompted for credentials. Try that in Access 2007 and above and you will be met with the following error:

> You do not have the necessary permissions to use the ‘.mdb’ object. Have your system administrator or the person who created this object establish the appropriate permissions for you.

Yep,  thanks error.

The short of it is, modern Access versions are more verbose. They need to be pointed in which direction to look for credentials.

To do so, create a shortcut file pointing to the following:

```plain
"c:\Path\To\MSACCESS.EXE" ".mdb" /wrkgrp ".mdw" /user
```

Add in the path to your version of Access, path to the .mdb database file and associated .mdw workgroup security file like the below example:

```plain
"C:\Program Files\Microsoft Office 15\root\office15\MSACCESS.EXE" "c:\temp\test.mdb" /wrkgrp "c:\temp\test.mdw" /user
```

Once created, the database will be accessible and you will be prompted for credentials when launched from the new shortcut.
