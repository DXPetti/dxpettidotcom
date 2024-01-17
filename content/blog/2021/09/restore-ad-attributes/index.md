---
title: "Restore AD Attributes"
date: "2021-09-20"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "active-directory"
  - "adds"
  - "powershell"
  - "sysadmin"
  - "technology"
coverImage: "restoreattrib_cover.jpg"
---

Back in the year that is one long COVID induced fog, I wrote about how one can [mount a old copy of an Active Directory Domain Services database]({{< ref "blog/2020/05/mounting-an-active-directory-database-backup" >}}) from something such as a file level backup of a DC. The impetus for this was a script that wreaked havoc on a large number of end users attributes in the AD object. Restoring from the AD recycle bin wouldn't help in this case as the object itself isn't deleted. Therefore, we need to selectively copy attributes from an old copy of the object.

Fortunately for you fellow reader, I have prepared a PowerShell script to do just that. If you want to skip the breakdown of the script, scroll down the bottom of this post. Otherwise, _"come with me if you want to live"_ ... and by live, I mean save some script running butts.

First up, lets set some mandatory parameters that point us to the mounted old AD database, the current/live AD database, the OU path of the affected users and lastly, a location to save a copy of the affected users properties prior to the restore (always have a plan B):  
  
```powershell
[Parameter(Mandatory=$true,
           ValueFromPipelineByPropertyName=$false,
           Position=0)]
[string]$OldAd,
 
[Parameter(Mandatory=$true,
           ValueFromPipelineByPropertyName=$false,
           Position=1)]
[string]$NewAd,
 
[Parameter(Mandatory=$true,
           ValueFromPipelineByPropertyName=$false,
           Position=2)]
[string]$AdOuPath,
[Parameter(Mandatory=$true,
           ValueFromPipelineByPropertyName=$false,
           Position=3)]
[string]$BkpPath
```

Next up, we are going to begin the grunt of the work by parsing our live AD environment and grab our list of users, export them to CSV for our plan B and use that list to build an array:

```powershell
Begin
{
    # Build list of Users
    Get-ADUser -Filter * -SearchBase $AdOuPath -Server $NewAd | Select samaccountname | Export-Csv -Path $BkpPath\Users.csv -NoTypeInformation
    $UserList = Import-Csv -Path $BkpPath\Users.csv
}
```

Continuing that theme, we are going to punch through each user in the list, grab all the properties from their object and write them out to individual text files for later use if necessary:

```powershell
Process
{
    foreach ($User in $UserList)
    {
        #Backup First
        Get-ADUser -Identity $User.SamAccountName -Properties * -Server $NewAd | Out-File "$BkpPath\$($User.SamAccountName)_before.txt"
```

Now on to the good bits...

![](images/200.gif)

We are doing the same thing as before but against the mounted old AD database and write the properties of interest to us (in the example, ```extensionAttribute1-6,9,13-14``` and ```publicDelegates/BL``` properties) to a ordered hash table:

```powershell
#Get Old Values
$OldProps = Get-ADUser -Identity $User.SamAccountName -Properties * -Server $OldAd
 
#Build Hash Tables
[hashtable]$OldValues = [ordered]@{
    extensionAttribute1 = $OldProps.extensionAttribute1
    extensionAttribute2 = $OldProps.extensionAttribute2
    extensionAttribute3 = $OldProps.extensionAttribute3
    extensionAttribute4 = $OldProps.extensionAttribute4
    extensionAttribute5 = $OldProps.extensionAttribute5
    extensionAttribute6 = $OldProps.extensionAttribute6
    extensionAttribute9 = $OldProps.extensionAttribute9
    extensionAttribute13 = $OldProps.extensionAttribute13
    extensionAttribute14 = $OldProps.extensionAttribute14
    publicDelegates = [array]$OldProps.publicDelegates
    publicDelegatesBL = [array]$OldProps.publicDelegatesBL
}
```

As it says on the tin, we are taking the properties saved within the hashtable, going through them one by one and setting them on the AD object in the live AD database:

```powershell
#Set Old Values
 
foreach ($O in $OldValues.GetEnumerator())
{
    Set-ADUser -Identity $User.samaccountname -Add @{$($O.Key)=$($O.Value)} -Server $NewAd -Verbose
}
```

At this point, the objective has been met but we have some house keeping to finish up the script. We export out all the properties of each user to text files, now featuring our newly set properties so we can do before and after comparisons with ease and we clear out a variable containing the hash table:

```powershell
#Export New Values
Get-ADUser -Identity $User.SamAccountName -Properties * -Server $NewAd | Out-File "$BkpPath\$($User.SamAccountName)_after.txt"
 
#Reset Hash Tables
$OldValues.Clear()
```

That's it. Only thing left to do is wrap it all up as a function and tie and nice PowerShell themed bow around it for sharing with the wider world.

Hopefully this helps if you ever find yourself in a similar scenario as we did. Scroll on down for the shiny, boxed up version.

_Godspeed sysadmin_

{{< gist DXPetti 670107f2944aad9d0579d722313d9b6b >}}
