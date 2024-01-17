---
title: "Bulk creation of Active Directory User accounts via Powershell v2"
date: "2012-01-22"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "active-directory"
  - "powershell"
  - "sysadmin"
  - "technology"
---

Most system administrators that I know has the burden (or the joy if you like a challenge) of creating and managing large quantities of users in Active Directory. I, myself face creating 150+ at the start of every year and I for the most part we have developed a new way to do it every year.

This year I chose to taken it upon myself to come up with the method and couldn't think of a better solution than Powershell. Considering all the work Microsoft seems to be putting behind this scripting & command engine it couldn't hurt to try.

The script I cobbled together is VERY basic (as I have practically zero knowledge of PS) but worked for me, hopefully it can be of use to anyone else.

## Requirements

- Powershell v2
- Active Directory Module for Powershell (installed by default on WK8R2 Domain Controllers or available in the latest RSAT tools)

## Steps

```powershell
import-csv .\import.csv | %{new-aduser -Name $_.Name -DisplayName $_.DisplayName -GivenName $_.GivenName -SamAccountName $_.SamAccountName -UserPrincipalName $_.UserPrincipalName -Description $_.Description -Surname $_.Surname -Path ‘OU=example,DC=domain,DC=com' -CannotChangePassword $false -ChangePasswordAtLogon $false ; Set-ADAccountPassword -identity $_.SamAccountName -NewPassword (ConvertTo-SecureString -AsPlainText $_.AccountPassword -Force) -Reset ; Enable-ADAccount -identity $_.SamAccountName}
```

Save the above code, paste into your favourite text editor and save as a ```.ps1``` (Powershell Script).

Next, create ```import.csv``` in the same directory as the script file and open it up in Excel. Now lets get the following headers inserted:

| Name | DisplayName | GivenName | SamAccountName | UserPrincipleName | Description | Surname | AccountPassword
|---|---|---|---|---|---|---|---|

All that is left is to fill in the csv with your data from where-ever that may be and run the script.

If I ever get around to it I hope to improve the script so that all that is required is _GivenName, Surname & SamAccountName_ as the rest should either be generated from the 3 listed or grabbed from the environment _(UserPrincipleName_). Currently I perform a bit of Excel formula magic to do this but it adds time and that's what we are trying to cut down right? I would also like to be able to add group memberships in the future but one thing at a time.

Hopefully that saves any SysAdmin's a day on trying to workout their own solution, if not, it certainly was a fun experience and a achievement for the day!
