---
title: "Bypass Offline Files With This One Trick"
date: "2020-05-25"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "csc"
  - "offline-files"
  - "sysadmin"
  - "technology"
  - "windows"
  - "windows-10"
  - "windows-8"
  - "windows-server"
  - "windows-server-2012-r2"
coverImage: "offlinefiles_cover.jpg"
---

Ever since moving to modern operating systems such as Windows 8 and beyond, I have found the Offline Files feature in the enterprise to be less and less of a value bringer and more of a support burden for the front line.

You only have to do a bit of Googling with keywords such as "Offline Files and DFS" to see endless amounts of frustrated fellow SysAdmin's and Engineers struggling with this technology aimed at bygone years of road warriors rocking their corporate device with point to point VPN profiles. The hybrid and distributed nature of today user is definitely worlds apart.

With that said, a problem is still a problem and as such, requires analysis, investigation and diagnosis.

When it comes to investigating Offline Files, it some times can be difficult to validate whether a users experience is due to Offline Files or not.

Until now.

For example, if your user in _contoso.co_ domain has a network drive (z:\ drive) mapped to a UNC path of ```\\fileserver\share```. This user says they find the contents of said drive to not be the same as the minute/hour/day before. They can only see a subset of the drive contents.

Sounds like Offline Files huh?

A very quick way to validate this is to access the UNC path with a special hidden share called ```$NOCSC$``` or **No C**lient **S**ide **C**aching. Client Side Caching being the Offline Files cache repository for Windows.

Just pop that share name at the end of server name, or in the case of DFS shares, at the end of your fully qualified domain name and you will be accessing the live contents, even with Offline Files enabled.

For the example discussed previously, that would look like:

```cmd
\\fileserver$NOCSC$\share or
\\contoso.co$NOCSC$\dfsroot\share
```

Too sweet huh!

Happy wrestling with Offline Files :)
