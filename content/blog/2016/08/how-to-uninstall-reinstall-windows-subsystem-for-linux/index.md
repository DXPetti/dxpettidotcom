---
title: "How to uninstall & reinstall Windows Subsystem for Linux"
date: "2016-08-27"
categories: 
  - "tech"
tags: 
  - "linux"
  - "technology"
  - "ubuntu"
  - "windows"
  - "windows-10"
  - "wsl"
coverImage: "wsl_cover.jpg"
---

Now that the Windows Subsystem for Linux (WSL) has been [released to the masses]({{< ref "blog/2016/08/bash-has-arrived-for-all-in-windows-10" >}}) in Windows 10 Anniversary Update and you have have installed the feature and had a play, What happens if you want to start from scratch and reset everything to a fresh state? This is something we often do in IT, especially when troubleshooting an issue. Instead of going into Programs & Features in Control Panel there is a very quick command you can run to perform the reset.

```cmd
lxrun /uninstall
```

This will uninstall the WSL environment from your workstation. Combine it with:

```cmd
lxrun /install
```

to reinstall the WSL environment and you are all reset.

You can also set root as the default user for WSL at the install phase with the following switch:

```cmd
lxrun /install /y
```

Alternatively, you can define your own user at install with:

```cmd
lxrun /install /setdefaultuser <username> /y
```

Note: if you omit the ```/y``` switch when defining your own username then you will be prompted to set a password for said user.

Super simple stuff! Go forth and get knee deep in the world of Linux, Windows Administrators
