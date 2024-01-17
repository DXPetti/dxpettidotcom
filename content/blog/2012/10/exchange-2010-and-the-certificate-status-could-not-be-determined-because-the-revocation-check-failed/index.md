---
title: "Exchange 2010 and \"The certificate status could not be determined because the revocation check failed\""
date: "2012-10-20"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "certificate"
  - "exchange-2010"
  - "proxy"
  - "ssl"
  - "sysadmin"
  - "technology"
---

On Friday while I was preparing our new Exchange 2010 VM for coexistance with our current Exchange 2007 physical box (more on that later) I ran into a annoying snag. Upon importing the PFX export of our current 3rd party wildcart certificate from IIS the Exchange 2010 management console threw me the below error while processing the certificate:

> The certificate status could not be determined because the revocation check failed

Seemed quite bizare at first. I knew the VM had internet connectivity. After trawling the internet I discovered that Exchange 2010 uses the WinHTTP interface to communicate with the internet rather than any settings defined in Control Panel>Internet Settings. This means if you are behind a forward proxy like Squid or TMG chances are the management console won't work quite right.

The issue is easy to resolve. Drop down to a administrative command prompt or powershell and type the following:

```cmd
netsh winhttp set proxy proxy-server="http=myproxyserver" bypass-list="myexchangeserver"
```

Where ```myproxyserver``` is your forward proxy DNS name or IP and ```myexchangeserver``` is your Exchange server DNS name (I used FQDN but NETBOIS should be fine) or IP.

Once you close down the management console and fire it back up your 3rd Party SSL certificate should now be verified.

This error is detailed in Microsoft KB Article [979694](http://support.microsoft.com/default.aspx?scid=kb;en-us;979694&sd=rss&spid=13965) and also on a Twitter friend ([@exchservpro](http://twitter.com/exchservpro)) of mines blog post [Exchange 2010 Certificate Revocation Checks and Proxy Settings](http://exchangeserverpro.com/exchange-2010-certificate-revocation-checks-and-proxy-settings "Exchange 2010 Certificate Revocation Checks and Proxy Settings").
