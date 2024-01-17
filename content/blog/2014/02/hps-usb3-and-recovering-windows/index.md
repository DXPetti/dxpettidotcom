---
title: "HP's + USB3 and Recovering Windows"
date: "2014-02-10"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "hp"
  - "sysadmin"
  - "technology"
  - "usb3"
---

Recently I started a Computer Science course through EdX (more on that in a future post) and found I liked to flesh out some of my thoughts via pen + pad. Not being a fan of wasting paper (whether it ends up in recycled or not) I opted to swap out my current notebook; a HP Folio 13 Ultrabook, for a notebook that has digitizer and pen support.

After finding a buyer for my HP I quickly went about cleaning up the Notebook and restored the operating system back to the factory preset, or at least I tried to. Unfortunately, when I loaded up the (one time only creation) recovery media via a external USB DVD-ROM I was presented with the message:

> You are not able to restore this system with the media.

_Uuuuuur what!_

I knew the recovery media was the correct one. I labelled the discs as soon as they were created. No mistake there. My next thought wandered to (now) faulty discs. After all, DVDs are susceptible to damage quite easily. If that was the case my only option would be to contact HP and order (at my cost) new recovery discs. I was on a short time frame, the buyer was expecting the machine in the next few days. I had to find another way.

After a little researching of the error message I found it was a quite common error with Ultrabooks. It seems, the USB3 port was the source of trouble. I had heard of early USB3 chipsets causing bluescreens and general havoc with machines so this didn't seem too far fetched. Luckily, my Folio 13 has a USB2 as well and low and behold, once the external USB DVD-ROM was connected via the USB2 port I was successfully able to restore my machine to factory defaults.

For reference, the USB3 chipset in my HP was of a _Fresco Logic USB 3.0 Host Controller_ variety. Be wary if you too, try to use it to recovery your operating system in conjunction with a external USB DVD-ROM.
