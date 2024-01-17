---
title: "Unable to log into vCenter Appliance via Web"
date: "2020-04-20"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "linux"
  - "photonos"
  - "sysadmin"
  - "systemd"
  - "technology"
  - "vcenter"
  - "vmware"
coverImage: "vcentersvc_cover.jpg"
---

With everyone scrambling to patch the [latest vulnerability](https://www.vmware.com/au/security/advisories/VMSA-2020-0006.html) to hit the VMware vCenter product, I came across some strange behaviour from our very own vCenter appliance running 6.7. When attempting to log into the web gui of the appliance, I was repeatedly getting a bad credentials error message.

I went round and round in circles trying to confirm I had the correct credentials, testing against vSphere etc... Eventually I started to suspect the issue was with the appliance itself.

Turned out I was on the right track - a service that powers the web gui was disabled on the appliance.

If you suspect something similar from your appliance, do the following to confirm and resolve:

1. Log into your appliances IP address, DNS or FQDN via SSH and login as ```root```
2. Run ```systemctl status applmgt``` - This should return a status of _DEAD_
3. Run ```systemctl list-unit-files``` - Look for our _applmgt_ service. It should be set to _Masked_.

For reasons unknown, the required service's unit files (the configuration files of the service) have been symlinked to /dev/null. This is otherwise known as Masking. Masking is the Linux equivalent to disabling a service. This is only done if you want a service to not run under any circumstances (including at boot and manually by a user). We need to remove this symlink and restart the service.

1. Run ```systemctl unmask applmgt```
2. Run ```systemctl list-unit-files``` - Look for our _applmgt_ service once more. This time, it should be set to _Enabled_
3. Run ```systemctl restart applmgt``` to kick off the service.

Now retry logging into the vCenter appliance's web gui. With any luck, you should be back to normal.
