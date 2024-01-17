---
title: "Migrated your mailbox to Exchange 2010, ActiveSync connection stopped? You might be a Domain Administrator"
date: "2012-11-29"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "active-directory"
  - "activesync"
  - "exchange"
  - "exchange-2010"
  - "sysadmin"
  - "technology"
---

Came across an interesting little bug which probably would have gone unnoticed if I never migrated my mailbox to our new Exchange 2010 mail server.

After performing a local move request and successfully migrating my mailbox across my exchange account would no longer connect on my Google Nexus 7 _(great tablet by the way)_. Access via Outlook 2010 continued to work as did access via the Outlook Web App. only the connection to my Nexus 7 (which is performed via ActiveSync) ceased to work.

I did some digging and found that my user account in Active Directory had some broken ACL inheritance and thus did not have the **Exchange Servers** group tied to my account. This prevents our shiny new Exchange 2010 server from updating the ActiveSync information in the user object and thus my Nexus 7 is probably still pointing to the old server.

Now when I say broken, I do mean broken with a _by design_ tacked on the end.

Because my account is a part of the **Domain Administrators** security group, inheritance of security permissions is removed to protect them (from any silly shenanigans a person with too much power might cause). Let's bring back the inheritance;

1. Open up **Active Directory Users and Computers** MMC snap-in
2. Go to **View**\>**Advanced Features** to enable all the really cool stuff
3. Find the affected user object (right-click on the domain and click **Find** is the quickest)
4. Right-click on the user object followed by clicking **Properties**
5. In the new window click on the **Security** tab followed by clicking on **Advanced**
6. Tick the box next to **Include inheritable permissions from this object's parent** and click **Apply**

And you're done! Just like that, mail should flow back on your favourite ActiveSync device

If you want to keep the put back up the safety barrier you can happily un-tick the box as it was previously now that the necessary permissions have inherited to the user object (remember to click **Add** on the prompt that will follow to keep your current and correct permissions).
