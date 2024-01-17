---
title: "LDAP properties not updated with NSCD"
date: "2021-10-04"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "ldap"
  - "linux"
  - "nscd"
  - "sssd"
  - "sysadmin"
  - "technology"
coverImage: "nscd_cover.jpg"
---

A colleague of mine shared with me a issue they were having with LDAP properties such as group memberships not being updated in a timely manner on some RHEL7 hosts that were running NSCD.

Not having any experience with NSCD, only SSSD for integrating Linux hosts in a enterprise LDAP environment for identity management, I madly started my google-fu engine and got to work.

Fortunately for you the reader, we can save you some time and get you the details you need.

## Verify

In our example of group memberships, the first thing we are going to do is making a modification externally to the Linux host to a LDAP user's group memberships. Use whatever tool you typically use.

Next, we are going to run the following command on the Linux host to see if it get's the changes:

```bash
getent group GROUPNAMEHERE
```

```getent``` allows us to query Name service switch databases for our LDAP group that we just modified. Hopefully the output displays the group membership prior to your change and verifies the issue.

## Validate

Now that we have verified the issue lies within the path of the Name server > LDAP, one of the intermediate points within this path is the **Name service cache daemon** or **NSCD**. Potentially, if the time-to-live on the NCSD cache is quite long then our host maybe query old and invalid data. Let's validate this by clearing said cache with:

```bash
sudo nscd --invalidate=group
```

Now that we have cleared the group-specific cache, lets verify again what our host can see with a:

```bash
getent group GROUPNAMEHERE
```

Hopefully we now see the group membership updated to include the changes we made. It did? Great. Now we have our root cause.

## Resolve

Okay, we have a stale cache problem. How do we go about preventing this in the future? Let's open up, with your favourite text editor, the NSCD configuration file with:

```bash
sudo nano /etc/nscd.conf
```

You should be presented with something like the following:

```plain
# This is a comment.

    logfile                 /var/log/nscd.log
    threads                 6
    server-user             nobody
    debug-level             0

    enable-cache            passwd          yes
    positive-time-to-live   passwd          600
    negative-time-to-live   passwd          20
    suggested-size          passwd          211
    check-files             passwd          yes

    enable-cache            group           yes
    positive-time-to-live   group           3600
    negative-time-to-live   group           60
    suggested-size          group           211
    check-files             group           yes

    enable-cache            hosts           yes
    positive-time-to-live   hosts           3600
    negative-time-to-live   hosts           20
    suggested-size          hosts           211
    check-files             hosts           yes
```

In this example, there are two values we are interested in the most, the positive TTL for group and the negative TTL:

```plain
    positive-time-to-live   group           3600
    negative-time-to-live   group           60
```

Positive TTL is the number of seconds after which a cached entry is removed from the cache and Negative TTL is the number of seconds after which entry marked as "not existent" is removed from the cache.

While not recommended, if it's preferable to turn off caching altogether, adjust the following value to false:

```plain
    enable-cache            group           yes
```

Tune those values to something that matches your environments needs, save the file and restart the daemon to bring them into effect with:

```bash
systemctl restart nscd
```

or if you are not using systemd:

```bash
service nscd reload
```

With the daemon restarted and your changes in affect, repeat the verification step above and then adjust config as needed until you hit that cache sweet spot.
