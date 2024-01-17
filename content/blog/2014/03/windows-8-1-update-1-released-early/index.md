---
title: "Windows 8.1 Update 1 released early"
date: "2014-03-07"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "sysadmin"
  - "technology"
  - "windows-8-1"
---

If you are not already aware, Microsoft have been preparing a whole host of improvements to Windows 8.1 entitled "Update 1" (creative huh!). Most of these changes are based around making Windows 8.1 more friendly to those of us using a traditional PC such as a desktop or non-touch notebook. If you are one of those people, and a little daring, there are two ways to get the update earlier than the scheduled April release.

## Method 1 - Download and Apply Windows Update packages:

| KB | Arch | URL |
|---|---|---|
| KB2919442 (Preparation Package) | x86 | download.windowsupdate.com/c/msdownload/update/software/crup/2014/02/windows8.1-kb2919442-x86_94ee3d715e732ed28c64b8096327375a35f5d211.msu |
|  | x64 | download.windowsupdate.com/c/msdownload/update/software/crup/2014/02/windows8.1-kb2919442-x64_f97d8290d9d75d96f163095c4cb05e1b9f6986e0.msu |
|  | ARM | download.windowsupdate.com/c/msdownload/update/software/crup/2014/02/windows8.1-kb2919442-arm_506ed7113697c597c2859d295d562fa4311834ec.msu |
| KB2939087 (Preparation Package) | x86 | download.windowsupdate.com/d/msdownload/update/software/crup/2014/03/windows8.1-kb2939087-x86_922410536df105c716a323b0a67956988aa17f40.msu |
|  | x64 | download.windowsupdate.com/d/msdownload/update/software/crup/2014/03/windows8.1-kb2939087-x64_89d4949cd698c645c7a6e96a012eecab44d5c5e1.msu |
|  | ARM | download.windowsupdate.com/d/msdownload/update/software/crup/2014/03/windows8.1-kb2939087-arm_53005551079dfe5ad4d03f85768801c794cedb60.msu |
| KB2919355 (Update 1 Package) | x86 | download.windowsupdate.com/c/msdownload/update/software/crup/2014/02/windows8.1-kb2919355-x86_de9df31e42fe034c9a763328326e5852c2b4963d.msu |
|  | x64 | download.windowsupdate.com/d/msdownload/update/software/crup/2014/02/windows8.1-kb2919355-x64_e6f4da4d33564419065a7370865faacf9b40ff72.msu |
|  | ARM | download.windowsupdate.com/c/msdownload/update/software/crup/2014/02/windows8.1-kb2919355-arm_a6119d3e5ddd1a233a09dd79d91067de7b826f85.msu |
| KB2932046 (Supplement Package) | x86 | download.windowsupdate.com/c/msdownload/update/software/crup/2014/02/windows8.1-kb2932046-x86_bfd8ca4c683ccec26355afc1f2e677f3809cb3d6.msu |
|  | x64 | download.windowsupdate.com/d/msdownload/update/software/crup/2014/02/windows8.1-kb2919355-x64_e6f4da4d33564419065a7370865faacf9b40ff72.msu |
|  | ARM | download.windowsupdate.com/c/msdownload/update/software/crup/2014/02/windows8.1-kb2932046-arm_fe6acf558880d127aef1a759a8c2539afc67b5fb.msu |
| KB2938439 (Supplement Package) | x86 | download.windowsupdate.com/c/msdownload/update/software/crup/2014/03/windows8.1-kb2938439-x86_ac9aca7e41c8e818edbea0a8026189ee086f7aa2.msu |
|  | x64 | download.windowsupdate.com/c/msdownload/update/software/crup/2014/03/windows8.1-kb2938439-x64_3ed1574369e36b11f37af41aa3a875a115a3eac1.msu |
|  | ARM | download.windowsupdate.com/d/msdownload/update/software/crup/2014/03/windows8.1-kb2938439-arm_4a536d9ddcd9993cbe4fbc309ebd50a18d65f954.msu |
| KB2937592 (Supplement Package) | x86 | download.windowsupdate.com/d/msdownload/update/software/crup/2014/02/windows8.1-kb2937592-x86_96a3416d480bd2b54803df26b8e76cd1d0008d43.msu |
|  | x64 | download.windowsupdate.com/c/msdownload/update/software/crup/2014/02/windows8.1-kb2937592-x64_4abc0a39c9e500c0fbe9c41282169c92315cafc2.msu |
|  | ARM | download.windowsupdate.com/c/msdownload/update/software/crup/2014/02/windows8.1-kb2937592-arm_860c83a0cccc0519111f57a679ae9f9d071315e5.msu |

Grab each MSU package for each KB article for your flavour of Windows and apply in top to bottom order as shown above.

## Method 2 - Edit the Registry:

1. Open up Windows Update and apply any waiting updates

2. Open up ```REGEDIT``` and go to the following key

   _HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows_  

3. Now create a new key inside and name it

   ```SHWindowsPoolS14```  

4. Restart your PC

5. Check Windows Update for Update 1 Package

As always, your mileage may vary so tread carefully and enjoy the update!

## Update

There are reports Microsoft have stopped the registry tweak from working and removed the files from Windows Update. Fear not, get the MSU package files from a MEGA mirror below:

- x86: https://mega.co.nz/#!nRAWFIqJ!h_frDQca2_Uds8_Dqt5wZoZN48OqDgO4PJ3K8LnOAkE`

- x64: https://mega.co.nz/#!TQpwWZqL!iNE8vjG4xrqASpDv_wyADTxkbzH2xZG9I5o8lg35Nyw`

- ARM: https://mega.co.nz/#!mF5CkIZS!fXPXoCbK3PhKG6O42nzw1w0bWwahboCWM4YHXvhmIT8`

Thanks to the [My Digital Life forum](http://forums.mydigitallife.info/threads/52611-DISCUSSION-Windows-8-1-Spring-2014-Update-(Windows-Feature-Pack)-RTM/page82?p=882751&viewfull=1 "DISCUSSION: Windows 8.1 Spring 2014 Update RTM") for the links
