---
title: "Mounting An Active Directory Database Backup"
date: "2020-05-18"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "active-directory"
  - "adds"
  - "dsamain"
  - "esentutl"
  - "sysadmin"
  - "technology"
coverImage: "restoread_cover.jpg"
---

We've all been there right; accidentally modified a Active Directory object that you weren't meant to. A innocent looking script, ran away and changed more objects than you thought it would. It's okay, these things happen.

But how does one go about restoring attributes of a Active Directory object? As it isn't a full on deletion of the object, Active Directory Recycle Bin cannot help you here. Grin and bear it? That's not going to cut it when its a VIP user.

If we take a step back and look at Active Directory objectively (ha!), it is just a database, no? If we had some method of restoring a old copy of the database (hope you do your nightly backups) side by side, we can in theory restore attributes selectively to the current version of the object right?

That's exactly what we are going to do.

### Restore a copy of the AD DS Database

So go to your backup method of choice, open a file system level restore from one of your Domain Controllers, grab the Active Directory database, ```NTDS.dit```, along with the log files starting with a **edb** prefix, (you'll see why later) from ```C:\Windows\NTDS\``` and place them in ```C:\temp\``` on a running DC.

### Clean the copy of the AD DS Database

If your backup method didn't make use of Volume Shadow Copy service built into Windows Server, chances are that the database isn't in a clean state. What do we mean by clean state? There were changes to the database that has not been committed to the database.

Thankfully, it's fairly straight forward to perform said clean.

1. On the DC where you restored a copy of the database, open up a administrative command prompt and navigate to said copy.
2. Run the following:

```cmd
esentutl /r edb
```

This is why we need those log files starting with the edb prefix. The above will analyse the edb log files and commit any changes that are requiring committal.

3. After the above has completed succesfully, lets run:

```cmd
esentutl /g ntds.dit
```

to perform a integrity check of the database

4. Last but not least, run:

```cmd
esentutl /p ntds.dit
```

to repair the database

The database should now be in a state to be mounted so we can start recovering attributes and other data from it.

### Mounting the AD DS database

We're in the endgame now folks and lucky for us, this is the most straight forward part.

We have a clean version of the database which contains the data we want to retrieve on a working Domain Controller. Now we are going to mount it.

From the same administrative command prompt, enter:

```cmd
dsamain /dbpath ntds.dit /ldapport 777
```

That final number is a number of your choosing, just make sure its not the same port number that your DC is currently using to serve up the AD DS database.

Once you run the above, it will launch a secondary window with the current status of the mount. Do not close this window as it will also unmount the database copy.

### Access the mounted AD DS Database

So we've mounted a copy of the database, now what?

The world is your oyster; any tool that you previous used to manage, maintain or otherwise consume your AD DS Database is usable with the mounted database. All you need to do is specify the port we used to mount it.

For instance, if I wanted to get a AD user's object with Powershell, I'd simply do:

```powershell
Get-ADUser -Identity JoeBlogs -Properties * -Server dc01.contoso.co:777
```

Hell, you could open Active Directory Users & Computers management GUI and point to the same server/port combo to wander though your point-in-time database.

For my next magic trick, [I'll show you how I utilised PowerShell]({{< ref "blog/2021/09/restore-ad-attributes" >}}) and the above mounted database restore selected attributes while taking backups along the way.

Stay tuned!
