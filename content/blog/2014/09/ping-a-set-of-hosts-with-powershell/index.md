---
title: "Ping a set of hosts with Powershell"
date: "2014-09-04"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "ping"
  - "powershell"
  - "sysadmin"
  - "technology"
  - "test-connection"
  - "windows"
---

You may find yourself one day having to ping a set of machines on a regular basis. Rather than drop to a Command Prompt and type in ping followed by each and every host name, wait for the results and move on to the next host name I asked myself there has got to be a better way to do things.

In comes the Powershell command Test-Connection.

Test-Connection is essentially the Powershell equivalent of the Ping command but with all the scripting benefits of being a Powershell command as well as being very expandable and customisable.

For my example environment; I have a couple of test sites and couple of different roles in each site and my naming convention is based around this (hosts are named _site-role_). Often sites are spun up or brought down so I wanted the script to be modular rather than hard code the host names somewhere directly. Thus with this script there are three files in total:

1. ```ping.ps1``` _The powershell script shown below_
2. ```sites.txt``` _A list of the site names_
3. ```roles.txt``` _A list of role names_

So without further ado, below is the PowerShell code to grab and put into your ```ping.ps1``` file:

```powershell
#Set Variables for AD Sites, Workstation Roles + Collection
$sites = Get-Content sites.txt
$roles = Get-Content roles.txt
$collection = $()
#Grab Site Name
foreach ($site in $sites)
    {
    #Add the Role Name
    foreach ($role in $roles)
        {
            #Combine Site + Role to create DNS name "site-role"
            $server = "$site-$role"
            $status = @{ "ServerName" = $server; "TimeStamp" = (Get-Date -f s) }
            #Ping DNS name via PS command Test-Connection once
            if (Test-Connection $server -Count 1 -ea 0 -Quiet)
                {
                    $status["Results"] = "Up"
                }
            else
                {
                    $status["Results"] = "Down"
                }
            #Output status of ping along with timestamp
            New-Object -TypeName PSObject -Property $status -OutVariable serverStatus
            $collection += $serverStatus
        }
    }
```

The script is already commented out to explain things step by step but essentially, for each site in sites.txt, powershell will Test-Connection once for each role in roles.txt and output the result complete with Up/Down status, a timestamp and the hostname.

Give it a try in your environment. Maybe you can expand the script and get it to log to an Excel file?
