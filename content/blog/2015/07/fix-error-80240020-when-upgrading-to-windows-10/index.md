---
title: "Fix error 80240020 when upgrading to Windows 10"
date: "2015-07-29"
categories: 
  - "tech"
tags: 
  - "sysadmin"
  - "technology"
  - "windows"
  - "windows-10"
  - "windows-update"
---

So you [heard the news]({{< ref "blog/2015/07/windows-10-is-here" >}}) and decided to update to Windows 10 but your Windows Update threw an 80240020 error? No problemo, you are being time-blocked by Microsoft. Do the following for the quick fix:

1. Open up your Date & Time settings and disable update/synchronise with the internet
2. Change the date to the 30th of July
3. Navigate to **c:\Windows\SoftwareDistribution\Download** and delete the contents of this folder
4. Open up a Command Prompt window and enter the following:

```cmd
wuauclt.exe /updatenow
```

Now the Windows 10 update should be streaming down and installing in no time.
