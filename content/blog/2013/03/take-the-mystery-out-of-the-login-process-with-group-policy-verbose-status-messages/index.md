---
title: "Take the mystery out of the login process with Group Policy Verbose Status messages"
date: "2013-03-14"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "group-policy"
  - "sysadmin"
  - "technology"
---

Let's be honest, no one enjoys the Please Wait dialog while you login to your computer unless of course you are rocking a Solid State Disk drive and you barely see the thing. User's often ask "Why is it taking so long?", "What is it doing?" etc... Yes, we hear it often enough that sometimes we disregard the users query and reply with a throw-a-way line from the tool bag. But what happens when you really want to know what's going on during the login process behind the Please Wait banner? Lets solve this mystery with a little bit of Group Policy.

Either create a new policy of open a existing one that fits your Active Directory structure and navigate to the following policy:

**Computer Configuration>Administrative Templates>System>Verbose vs normal status messages**

Open up the policy and change to **Enabled** and click OK.

Once this applied has applied no longer will the end user see the Please Wait message, instead, they will see exactly what Group Policies are being processed on log on/log off as well as the other processes during startup and shutdown.

I applied this setting domain wide at my organisation because while IT now had much greater information when diagnosing a login or group policy issue (I cannot express how handy this information can be) but as a added bonus I found it had a great effect on the end user. No longer were they kept in the dark in regards to what's going on with their computer and they felt more in control, less agitated about the login process.

Try it and see if your end users notice the change. Information is power!
