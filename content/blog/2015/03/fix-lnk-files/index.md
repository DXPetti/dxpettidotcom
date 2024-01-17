---
title: "Fix .LNK and .EXE file associations"
date: "2015-03-16"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "malware"
  - "registry"
  - "sysadmin"
  - "technology"
  - "virus"
  - "windows"
  - "windows-7"
---

There are a number of malware out in the wild that like to change the association for .LNK (shortcut files) and .EXE (executable files) in Windows to something obscure or remove them entirely which makes it rather difficult to do daily task like say...open an application.

Even after the malware has been detected, destroyed and banished from the workstation, the damage it has created lingers on as to somewhat taunt the user.

But there is a way to restore these files to their original state without resorting to drastic measures like a rebuild.

Firstly, (with another user account or workstation if need be) open up Notepad and copy the code below and save as a .reg file _e.g. fixlnkexe.reg_

```ini
Windows Registry Editor Version 5.00
 
;For [.LNK] and [.EXE] - Windows 7
 
[HKEY_CLASSES_ROOT\.LNK]
@="lnkfile"
 
[HKEY_CLASSES_ROOT\.LNK\ShellEx\{000214EE-0000-0000-C000-000000000046}]
@="{00021401-0000-0000-C000-000000000046}"
 
[HKEY_CLASSES_ROOT\.LNK\ShellEx\{000214F9-0000-0000-C000-000000000046}]
@="{00021401-0000-0000-C000-000000000046}"
 
[HKEY_CLASSES_ROOT\.LNK\ShellEx\{00021500-0000-0000-C000-000000000046}]
@="{00021401-0000-0000-C000-000000000046}"
 
[HKEY_CLASSES_ROOT\.LNK\ShellEx\{BB2E617C-0920-11d1-9A0B-00C04FC2D6C1}]
@="{00021401-0000-0000-C000-000000000046}"
 
[HKEY_CLASSES_ROOT\.LNK\ShellNew]
"Handler"="{ceefea1b-3e29-4ef1-b34c-fec79c4f70af}"
"IconPath"=hex(2):25,00,53,00,79,00,73,00,74,00,65,00,6d,00,52,00,6f,00,6f,00,\
74,00,25,00,5c,00,73,00,79,00,73,00,74,00,65,00,6d,00,33,00,32,00,5c,00,73,\
00,68,00,65,00,6c,00,6c,00,33,00,32,00,2e,00,64,00,6c,00,6c,00,2c,00,2d,00,\
31,00,36,00,37,00,36,00,39,00,00,00
"ItemName"="@shell32.dll,-30397"
"MenuText"="@shell32.dll,-30318"
"NullFile"=""
 
[HKEY_CLASSES_ROOT\.LNK\ShellNew\Config]
"DontRename"=""
 
[HKEY_CLASSES_ROOT\lnkfile]
@="Shortcut"
"EditFlags"=dword:00000001
"FriendlyTypeName"="@shell32.dll,-4153"
"IsShortcut"=""
"NeverShowExt"=""
 
[HKEY_CLASSES_ROOT\lnkfile\CLSID]
@="{00021401-0000-0000-C000-000000000046}"
 
[HKEY_CLASSES_ROOT\lnkfile\shellex\ContextMenuHandlers\Compatibility]
@="{1d27f844-3a1f-4410-85ac-14651078412d}"
 
[HKEY_CLASSES_ROOT\lnkfile\shellex\ContextMenuHandlers\OpenContainingFolderMenu]
@="{37ea3a21-7493-4208-a011-7f9ea79ce9f5}"
 
[HKEY_CLASSES_ROOT\lnkfile\shellex\ContextMenuHandlers\{00021401-0000-0000-C000-000000000046}]
@=""
 
[HKEY_CLASSES_ROOT\lnkfile\shellex\DropHandler]
@="{00021401-0000-0000-C000-000000000046}"
 
[HKEY_CLASSES_ROOT\lnkfile\shellex\IconHandler]
@="{00021401-0000-0000-C000-000000000046}"
 
[HKEY_CLASSES_ROOT\lnkfile\shellex\PropertySheetHandlers\ShimLayer Property Page]
@="{513D916F-2A8E-4F51-AEAB-0CBC76FB1AF8}"
 
[-HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.LNK\UserChoice]
 
[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.LNK\OpenWithProgids]
"lnkfile"=hex(0):
 
[HKEY_CLASSES_ROOT\.EXE]
@="exefile"
"Content Type"="application/x-msdownload"
 
[HKEY_CLASSES_ROOT\.EXE\PersistentHandler]
@="{098f2470-bae0-11cd-b579-08002b30bfeb}"
 
[HKEY_CLASSES_ROOT\exefile]
@="Application"
"EditFlags"=hex:38,07,00,00
"FriendlyTypeName"=hex(2):40,00,25,00,53,00,79,00,73,00,74,00,65,00,6d,00,52,\
00,6f,00,6f,00,74,00,25,00,5c,00,53,00,79,00,73,00,74,00,65,00,6d,00,33,00,\
32,00,5c,00,73,00,68,00,65,00,6c,00,6c,00,33,00,32,00,2e,00,64,00,6c,00,6c,\
00,2c,00,2d,00,31,00,30,00,31,00,35,00,36,00,00,00
 
[HKEY_CLASSES_ROOT\exefile\DefaultIcon]
@="%1"
 
[HKEY_CLASSES_ROOT\exefile\shell\open]
"EditFlags"=hex:00,00,00,00
 
[HKEY_CLASSES_ROOT\exefile\shell\open\command]
@="\"%1\" %*"
"IsolatedCommand"="\"%1\" %*"
 
[HKEY_CLASSES_ROOT\exefile\shell\runas]
"HasLUAShield"=""
 
[HKEY_CLASSES_ROOT\exefile\shell\runas\command]
@="\"%1\" %*"
"IsolatedCommand"="\"%1\" %*"
 
[HKEY_CLASSES_ROOT\exefile\shell\runasuser]
@="@shell32.dll,-50944"
"Extended"=""
"SuppressionPolicyEx"="{F211AA05-D4DF-4370-A2A0-9F19C09756A7}"
 
[HKEY_CLASSES_ROOT\exefile\shell\runasuser\command]
"DelegateExecute"="{ea72d00e-4960-42fa-ba92-7792a7944c1d}"
 
[HKEY_CLASSES_ROOT\exefile\shellex\ContextMenuHandlers]
@="Compatibility"
 
[HKEY_CLASSES_ROOT\exefile\shellex\ContextMenuHandlers\Compatibility]
@="{1d27f844-3a1f-4410-85ac-14651078412d}"
 
[HKEY_CLASSES_ROOT\exefile\shellex\DropHandler]
@="{86C86720-42A0-1069-A2E8-08002B30309D}"
 
[HKEY_CLASSES_ROOT\exefile\shellex\PropertySheetHandlers]
 
[HKEY_CLASSES_ROOT\exefile\shellex\PropertySheetHandlers\PifProps]
@="{86F19A00-42A0-1069-A2E9-08002B30309D}"
 
[HKEY_CLASSES_ROOT\exefile\shellex\PropertySheetHandlers\ShimLayer Property Page]
@="{513D916F-2A8E-4F51-AEAB-0CBC76FB1AF8}"
 
[-HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.EXE\UserChoice]
 
[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.EXE\OpenWithProgids]
"exefile"=hex(0):
```

Under the user account which is affected, double-click on the registry file to merge the contents into the users and computers registry hives. If you are presented with an error stating not all parts could be successfully merged that is due to the current user account not being an administrator. To resolve this, run the same registry file under and administrator account to merge the rest of the file into the registry hives (it's vital to run the registry file under the affected account regardless if they are an administrator or not).

Next, open up a administrative command prompt and run the following command:

```cmd
del /F c:\Users\username\AppData\Local\iconcache.db\
```

Ensure to replace ```username``` with the affected users username.

Finally, restart the workstation and all should be well come the next login.

Peace has been restored.
