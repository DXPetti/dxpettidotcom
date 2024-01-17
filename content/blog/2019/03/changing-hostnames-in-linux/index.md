---
title: "Changing Hostnames in Linux"
date: "2019-03-04"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "debian"
  - "linux"
  - "raspberry-pi"
  - "raspbian"
  - "sysadmin"
  - "technology"
coverImage: "hostname_cover.jpg"
---

Recently I decided to give my two Raspberry Pi devices an overhaul + change in functionality. Swapped out my Raspberry Pi Zero from being the always on, ASIC mining, Pi-Hole wonder to a mobile lab for my ongoing Linux SysAdmin learning that I touched upon [here]({{< ref "blog/2018/10/lets-get-certified" >}}) by utilising the Zero's unique ability to be powered and network over the USB (more on that in a later post). Consequently, that put my Raspberry Pi A+ back into service.

Rather than just simply swap out the SD Card from the Zero, I wanted to start from scratch. Always a nicety in the world of IT, plus, it makes for another opportunity to get my hands dirty and perform tasks you often only do once. Set and forget if you will.

Well, one of those tasks is changing the hostname of the device to something with a bit more flavour than just 'raspberrypi' as it is by default in Raspbian.

Scratching my head, I had to remember what exactly the steps are to modify the hostname. Now that they are forefront of my mind, below is what is required:

{{< alert "circle-info" >}}
The below applies to Raspbian and thus other Debian or Debian derivative Operating Systems. Though it's safe to assume the vast majority of Linux Operating Systems will accept the below steps, YMMV.
{{< /alert >}}

Either as root, or elevate via ```sudo```; using your favourite text editor (in the below case, ```nano```) open the hosts file located in _/etc/_

```bash
sudo nano /etc/hosts
```

Modify the entry that lists the current hostname **together** with the IP address of 127.0.1.1 to state the new hostname, followed by saving and closing.

Next, once again, as root, or elevate via ```sudo```; using the text editor, open the hostname file located in _/etc/_

```bash
sudo nano /etc/hostname
```

Clear out the only entry in the file and enter the desired hostname, followed by a save and close.

Once complete, a quick reboot and you will have a newly named Linux host. Too easy.
