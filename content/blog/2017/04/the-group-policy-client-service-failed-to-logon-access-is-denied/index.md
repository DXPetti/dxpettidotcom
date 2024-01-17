---
title: "The Group Policy Client service failed to logon - Access is denied"
date: "2017-04-24"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "group-policy"
  - "sysadmin"
  - "technology"
  - "windows"
  - "windows-7"
---

Access is denied is a truly wonderful error.

On one side of the coin, it's an error; which automatically makes it bad. You are being prevented in doing some you need to do.

On the other side, it's very verbose, short, to the point and quickly narrows down where the problem lies.

Permissions... they're wrong. And in the case of the Group Policy Client service throwing an _Access is denied_ error then you should cast your eye upon the NTFS permissions of the users profile. Whether it be a mandatory profile or a roaming profile, there is a file or whole sections that don't have the correct permissions applied.

In the case that I came across, the users roaming profile **NTUSER.dat** file was missing permissions for the user themselves. A quick remove inheritence, copy existing permissions, reinherit permissions from above and the error was banished.

Just like the error, this resolution is short and to the point.
