---
title: "How to restore a tree of objects from the Active Directory Recycle Bin"
date: "2017-05-22"
series: ["Active Directory Recycle Bin"]
series_order: 2
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "active-directory"
  - "powershell"
  - "sysadmin"
  - "technology"
coverImage: "adrecycle_cover.jpg"
---

In a [previous post]({{< ref "blog/2013/04/how-to-setup-and-restore-a-object-from-the-active-directory-recycle-bin" >}}) I detailed how to setup the Active Directory Recycle Bin and restore single deleted objects back in your Active Directory. Restoring a whole tree of objects though (i.e. Organisational Unit and the objects inside it) is no easy process.

Thankfully [Microsoft via the community](https://technet.microsoft.com/en-us/library/dd379504(v=ws.10).aspx) have provided a example script to do this:

```powershell
Param (
 $lastKnownRDN,
 $lastKnownParent,
 $identity,
 $partition,
[switch] $includelivechildren,
[switch] $whatIf,
[switch] $verbose
)
 
###############################
##      Display Help         ##
###############################
 
function Display-Help {
""
"Usage:"
""
"Restore-ADTree  -lastKnownRDN "
"[-lastKnownParent ]"
"[-partition ]"
"[-includeLiveChildren]"
"[-whatif]"
"[-verbose]"
"OR"
""
"Restore-ADTree  -identity "
"[-partition ]"
"[-includeLiveChildren]"
"[-whatif]"
"[-verbose]"
""
"Examples:"
""
"Restore-ADTree -lastknownRDN Accounting"
""
"Restore-ADTree -lastKnownRDN Accounting -lastknownParent ""DC=CONTOSO,DC=COM"" "
""
"Restore-ADTree -identity b48290aa-e14f-4417-9c03-560a546d18b9"
""
"Restore-ADTree -identity ""OU=Accounting,DC=CONTOSO,DC=COM"" "
""
}
 
###############################
##     Validate Parameters   ##
###############################
 
if (!(($identity) -or ($lastKnownRDN))){
display-help
break
}
 
if (($identity) -and (($lastKnownRDN) -or ($lastKnownParent))){
display-help
break
}
 
####################################################
##     INCOMPLETE Get RDNType given objClass      ##
####################################################
 
Function Get-RDNType {
Param($objClass)
 
switch ($objClass){
 
"container"{Return "CN"}
"OrganizationalUnit" {Return "OU"}
default {Return "CN"}
}
}
 
############################################
##     Restore-Tree Recursive Function    ##
############################################
 
function Restore-Tree($strObjectGUID ,$strNamingContext, $bIncludeLiveChildren, $bWhatIf ,$strRestoredPrevParentDN)
{
      $objRestoredParent = $null
$objRestoredParent = get-adobject -identity $strObjectGUID -partition $strNamingContext
 
if ($objRestoredParent){
 
if (!($bWhatIf)){
 
Write-Host ""Not restoring live object $objRestoredParent.distinguishedName .""  -ForeGroundColor Yellow
 
} else {
 
Write-Host ""Will not restore live object $objRestoredParent.distinguishedName .""  -ForeGroundColor Yellow
 
}
 
$strLiveSearchBase = $objRestoredParent.distinguishedName
$RestoredDN = $objRestoredParent.distinguishedName
 
} else {
 
if (!($bWhatIf)){
 
Restore-ADobject -identity $strObjectGUID -partition $strNamingContext -errorVariable errRestore
 
if  (($errRestore)){
$objRestoredParent = get-adobject -identity $strObjectGUID -partition $strNamingContext -includeDeletedObjects
Write-Host ""Restore of object $objRestoredParent.distinguishedName failed.`n Error: $errRestore""  -ForeGroundColor Red
Exit Function
} else {
$objRestoredParent = get-adobject -identity $strObjectGUID -partition $strNamingContext
$RestoredDN = $objRestoredParent.distinguishedName
Write-Host ""Successfully restored object $objRestoredParent.DistinguishedName"" -ForeGroundColor Green
}
} else {
$objRestoredParent = get-adobject -identity $strObjectGUID -partition $strNamingContext -properties msds-lastknownRDN,lastKnownParent,whenChanged  -includeDeletedObjects
 
$RestoredDN = $(Get-RDNType($objRestoredParent.ObjectClass)) + "=" + $objRestoredParent.("msds-lastKnownRDN") + "," + $strRestoredPrevParentDN
Write-Host ""Will restore deleted object $RestoredDN""  -ForeGroundColor Green
if ($verbose){
Write-Host ""Deleted DN: $objRestoredParent.distinguishedName `n whenDeleted: $objRestoredParent.("whenChanged") ""
}
}
$strLiveSearchBase = $null
}
 
if (($strLiveSearchBase) -and ($bIncludeLiveChildren)) {
 
$strFilter = "(objectClass=*)"
 
        $objChildren = get-adobject -SearchScope onelevel -SearchBase $strLiveSearchBase -ldapFilter $strFilter  -ResultPageSize 300 -ResultSetSize 10000
 
        if ($objChildren -ne $null){
        foreach ($objChild in $objChildren)
{
Restore-Tree $objChild.objectGUID $strNamingContext $bIncludeLiveChildren $bWhatIf $RestoredDN
}
}
 
}
 
$strSearchBase = "CN=Deleted Objects,"+$strNamingContext
 
$strFilter = "(lastknownParent=" + $objRestoredParent.distinguishedName.Replace("\0","\\0") + ")"
 
        $objChildren = get-adobject -SearchScope subtree -SearchBase $strSearchBase -includedeletedobjects -ldapFilter $strFilter -ResultPageSize 300 -ResultSetSize 10000
 
        if ($objChildren -ne $null){
Write-Host ""Restoring deleted children of $RestoredDN""
        foreach ($objChild in $objChildren)
{
Restore-Tree $objChild.objectGUID $strNamingContext $bIncludeLiveChildren $bWhatIf $RestoredDN
}
}
}
 
######################
##  Main Function   ##
######################
 
$ErrorActionPreference = "SilentlyContinue"
 
if (!($partition)){
$strNamingContext = [string] (get-adrootDSE).defaultNamingContext
} else {
 
$strNamingContext = $partition
}
 
$strDelObjContainer = "CN=Deleted Objects,"+$strNamingContext
 
if ($identity){
 
$objSearchResult = get-adobject -identity $identity -partition $strNamingContext -includeDeletedObjects
 
} else {
 
$strFilter = "(msds-lastknownRDN=" + $lastKnownRDN + ")"
 
$objSearchResult = get-adobject -SearchScope subtree -SearchBase $strDelObjContainer -includedeletedobjects -ldapFilter $strFilter  -properties lastknownparent,whenChanged,isDeleted
}
 
If (!($objSearchResult))  {
Write-Host "Search for tree root returned 0 objects.Exiting without making changes.";Exit
} Else {
 
$objMeasure = $objSearchResult | Measure-Object
If ($objMeasure.Count -gt 1) {
Write-Host "Search for tree root returned more than one object.Rerun command and select one of below objects" -ForeGroundColor Yellow
foreach ($objRoot in $objSearchResult) { $objRoot}
break
}
}
 
if ($objSearchResult.isDeleted) {
 
$PrevParent = $objSearchResult.("lastKnownParent")
$bRootIsDeleted = $true
 
} else {
 
$PrevParent = $objSearchResult.distinguishedName
$bRootIsDeleted = $false
}
 
Restore-Tree $objSearchResult.objectGUID $strNamingContext $includelivechildren $whatIf $PrevParent -errorVariable errRestore
```

Copy the above into a Powershell Script file named **restoreadtree.ps1**. There are a number of parameters we can use to perform the restore depending on your knowledge of the deleted object.

If you know the exact path of the Organisational Unit:

```powershell
.\restoradtree.ps1 -identity ""OU=Admins,DC=DXPETTI,DC=COM""
```

This would restore the Organisational Unit "Admins" that sat at the top level of the DXPETTI.COM Active Directory domain.

Or if you couldn't remember where an OU sat but still remember the name:

```powershell
.\restoreadtree.ps1 -lastKnownRDN Admins
```

This would restore the Organisational Unit Admins despite not remembering the full path.

But what if you had multiple OUs with the same name? If you remember a part of the path then:

```powershell
.\restoreadtree.ps1 -lastKnownRDN Admins -lastKnownParent ""OU=Users,DC=DXPETTI,DC=COM""
```

The above would restore the "Admins" OU that sat under (directly or otherwise) the User OU in the root level of the DXPETTI.com Active Directory domain.

Happy AD Recycle Bin restoring!
