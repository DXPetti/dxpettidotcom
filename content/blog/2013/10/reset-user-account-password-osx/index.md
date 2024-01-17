---
title: "Reset user account password in OSX"
date: "2013-10-28"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "leopard"
  - "lion"
  - "mountain-lion"
  - "osx"
  - "snow-leopard"
  - "sysadmin"
  - "technology"
  - "tiger"
---

Although I hope most SysAdmin's don't forget their passwords, it sometimes can happen. Maybe you happened upon a 2nd hand machine and don't have any recovery media to reset the operating system to a clean state. These are situations that require you to reset a user account's password.

Most of my posts surround Microsoft operating systems but I'm going to switch it up today and show you how to reset a user account password on Apple's OSX.

### OSX 10.4 Tiger

1. With the machine off, power it on and hold the **Command** + **S** keys. This will engage the OSX's Single User Mode.

2. At the prompt, type ```sh /etc/rc``` and hit the **Enter** key.

3. Type ```passwd username``` replacing ```username``` with your chosen user account.

4. Enter and confirm a replacement password of your choosing at the prompt.

5. Now we can reset the machine and use the freshly reset password by typing ```reboot``` and hitting **Enter**.

### OSX 10.5 Leopard and above

1. With the machine off, power it on and hold the **Command** + **S** keys. This will engage the OSX's Single User Mode.

2. At the prompt, type ```mount -uw``` and hit the **Enter** key.

3. Next, type ```launchctl load /System/Library/LaunchDaemons/com.apple.DirectoryServices.plist``` and hit **Enter** to load the User Accounts from the local system.

4. Now type ```ls /Users``` to list all the User Accounts that were loaded in the previous step.

5. Finally we can reset the password on a chosen account by entering ```dscl . -passwd /Users/username password``` and replace ```username``` and ```password``` with your chosen user account to reset and a password of your choice.

6. Reset your system via the ```reboot``` command and your all (re)set.

Too easy. Now your forgetful mind or a second hand system won't be a roadblock to access to your OSX machine.

Fellow OSX users, let me know in the comments if the above no longer works in Mavericks, cheers!
