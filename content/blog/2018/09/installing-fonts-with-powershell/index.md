---
title: "Installing Fonts with PowerShell"
date: "2018-09-24"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "application-packaging"
  - "fonts"
  - "powershell"
  - "sccm"
  - "sysadmin"
  - "technology"
  - "windows"
  - "windows-10"
  - "windows-7"
coverImage: "fonts_cover.jpg"
---

Not normally the application packaging type but some time ago it fell to me to package up some open source fonts and deploy them via System Centre Configuration Manager across the enterprise. While this seems like a simple task at first, fonts are treated in a special way in Windows and we were also under strict legal instructions to comply with the open source licensing requirements of the font which were

- User must have the license terms & conditions displayed to them prior to install
- If the user accepts the terms & conditions, install plus deploy a copy of said terms & conditions to the endpoint

With this in mind and after a couple of trial and error bumps a long the road I came up with the below ```Install-Fonts.ps1``` Powershell script.

{{< gist dxpetti 0f4f22884c57dc934b26a4dc314eeb55 >}}

To use, initiate the script from the same directory as your font(s) file together with a text document labelled ```LICENSEFILE.txt``` which contains the terms & conditions. The contents of the text file should be pasted into the following line:

```powershell
## Vendor font license that will be displayed to the user for acceptence
$license = "LICENSE TEXT"
```

by replacing _LICENSE TEXT_ so it is displayed to the user to allow them to accept prior to the installation kicking off. The license name and version should also be added to the Powershell script in the following line:

```powershell
## Output License to user for acceptence 
$output = [System.Windows.Forms.MessageBox]::Show("$license
Do you agree to the above Terms & Conditions?"
 , "LICENSE NAME AND VERSION" , 4)
```

by replacing _LICENSE NAME AND VERSION_.

The above two elements could be made to be a bit more dynamic but hey, that's what version 2.0 is for :)
