---
title: "Enable VNC server via SSH on OSX"
date: "2013-11-04"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "osx"
  - "remote-desktop"
  - "screen-sharing"
  - "ssh"
  - "sysadmin"
  - "technology"
  - "vnc"
---

Continuing on with my [previous posts theme of OSX]({{< ref "blog/2013/10/reset-user-account-password-osx" >}}), the next handy guide will demonstrate how to enable the VNC server (Microsoft people, think RDP server) via SSH.

By default, SSH is enabled on OSX Server (SSH will need to be enabled on non Server builds) and is a great way of remotely administering a machine (OSX's UNIX background lends to this) but for the times you need a GUI, we will need the VNC server setup.

Fire up your favourite SSH client (putty is the choice of many) and connect to your OSX Server with your administrative level username and password.

Next, input the following to enable the VNC server. Be sure to change ```password``` to a password of your choosing:

```bash
sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -activate -configure -access -on -clientopts -setvnclegacy -vnclegacy yes -clientopts -setvncpw -vncpw password -restart -agent -privs -all
```

Now you will be able to use any VNC client to connect to your OSX machine.

If you would like to disable the VNC server, this also can be done via SSH with the following command:

```bash
sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -deactivate -configure -access -off
```

If you would like to enable access o the VNC server with the OSX machine's regular user accounts rather than a one size fits all VNC password the below, when entered in the SSH session will achieve just that:

```bash
sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -activate -configure -access -on -configure -allowAccessFor -allUsers -configure -restart -agent -privs -all
```

Now you can enjoy the beautiful OSX GUI without being  stuck in front of the machine.
