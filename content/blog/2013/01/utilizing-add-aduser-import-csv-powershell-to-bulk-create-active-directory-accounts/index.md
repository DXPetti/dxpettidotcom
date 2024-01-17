---
title: "Utilizing Add-ADUser & Import-CSV Powershell Cmdlets to bulk create Active Directory accounts"
date: "2013-01-25"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "active-directory"
  - "powershell"
  - "sysadmin"
  - "technology"
---

We've all been there...your company has just taken over or brought out another company and you have been given a list of new employees to receive network accounts or another example (and my reality) it is the start of a school year and you have a herd of new year 7 students that need network accounts.

Like all good sysadmin's, we are lazy. Why do something by hand when you can create a script or app to do the work on a large-scale. That is exactly what I have done.

You will find this time last year I tackled this same subject. I noted while [my first attempt]({{< ref "/blog/2012/01/bulk-creation-of-active-directory-user-accounts-via-powershell-v2" >}}) got the job done, there was plenty of additional things I wanted the script to do so I could be completely hands of when bulk creating the network accounts. Things like security group memberships or making the script handle passwords generation and User Principle Name generation automatically instead of me doing it by hand via Excel. In the name of progress I have achieved this and all in a nice annotated fashion so it's easy for others to implement in their environment.

## What do I need for this script to work?

- Powershell V3
- Active Directory Modules for Powershell

You can get the above a number of ways;

1. Windows Server 2012 Domain Controller
2. Windows 8 w/ latest [Remote Server Administration Tools](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-vista-windows-7-windows-8-windows-server-2008-windows-server-2008-r2-and-windows-server-2012-dsforum2wiki.aspx "Remote Server Administration Tools (RSAT) for Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, and Windows Server 2012 ")
3. Windows 7 Service Pack 1, Windows Server 2008 R2 SP1, Windows Server 2008 Service Pack 2 with the [WMF 3.0](http://www.microsoft.com/en-us/download/details.aspx?id=34595 "Windows Management Framework 3.0") update and the latest [Remote Server Administration Tools](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-vista-windows-7-windows-8-windows-server-2008-windows-server-2008-r2-and-windows-server-2012-dsforum2wiki.aspx "Remote Server Administration Tools (RSAT) for Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, and Windows Server 2012 ")

- CSV file with three columns;
   | Firstname | Lastname | Username |  
   |---|---|---|  

And here is the script, copy/paste into a text document and save with a .ps1 extension or copy/paste into your favourite Powershell IDE.

{{< gist dxpetti 586d54bd7fdf229fea44605fe516cea3 >}}

Pretty self explanatory thanks to the annotations but I will break down the key elements that you will need to fill in the blanks.

## Where is the CSV file?

```powershell
# Import list of Users From CSV into $Userlist
 
$UserList=IMPORT-CSV driveletter:\pathtofile.csv
```

Replace ```driveletter:\pathtofile.csv``` to the local path to the CSV file containing the user data 
{{< alert "circle-info" >}}
I recommend **CSV (MS-DOS)** when saving from Excel
{{< /alert >}}

## What is your Windows network domain name?

```powershell
# Build and define Domain name

$Domain="@yourdomainhere.com"
```

Replace ```yourdomainhere.com``` with the fully qualified domain name of your Windows domain network.

## Where is the user's Home directory going to be?

```powershell
# Build and define Home Directory path

$HDrive="\\uncpathtohomeshare\"
```

Replace ```\uncpathtohomeshare\``` with the UNC path to the user's individual file share for their home directory.
{{< alert "circle-info" >}}
There seems to be a bug that when the home directory is defined in Powershell it is not automatically created _unlike_ when it is manually defined in the Active Directory Users and Computers MMC snap-in thus the home directories will have to be created before hand (or use ```%username%``` along with your file server UNC path while multi-selecting the properties of accounts in Active Directory Users and Computers MMC snap-in).
{{< /alert >}}

## What organizational unit will the new accounts reside in?

```powershell
# Build and define which Organizational Unit to create User inside

$OU="OU=organizationunitofyourchoice,DC=yourdomainhere,DC=com"
```

This one may not be applicable to all but in my case I have all my users in a well organised structure and not a free for all in the default Organizational Unit of **Users**. To make use of this, replace ```OU=organizationunitofyourchoise,DC=yourdomainhere,DC=com``` with the Distinguished Name path of your Organizational Unit of choice _(tip: to find the DN path of an OU while in Active Directory Users and Computers snap-in go to **View>Advanced Features** then right-click on the OU and click **Properties**. In the new tab click on **Attribute Editor** and scroll down to **Distinguished Name** and copy/paste the value)_.

## What security groups will the new accounts require?

```powershell
# Add User to Security Groups

Add-ADPrincipalGroupMembership -Identity $Username -MemberOf "security group a","security group b"
```

Once again, this may not be applicable to all but in some cases you require all users to be a member of a  selection of security groups _(note: the default Domain Users group does not need to be specified and is implied)_. If you are one of those people, replace ```security group a``` and ```security group b``` and ```security group c``` with the display name of your security group(s).

I hope the above explains all the minimum things you need to input for the script to work. If you are adventurous there are plenty of things you can change-up _e.g. generate the username based of the firstname and lastname, how the password is generated, set the description field to Sales Staff etc..._ but I will leave that for you to decide.

The great thing about Powershell is how endless the possibilities are. You could use the script above to update rather than create a bulk amount of network accounts using the ```Set-ADUser``` cmdlet rather than ```Add-ADUser```. Use your imagination and run wild!

For more information on the ```Add-ADUser``` cmdlet head to [http://technet.microsoft.com/en-us/library/ee617253.aspx](http://technet.microsoft.com/en-us/library/ee617253.aspx) for a complete run down
