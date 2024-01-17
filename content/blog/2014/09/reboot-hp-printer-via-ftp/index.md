---
title: "Reboot HP Printer via FTP"
date: "2014-09-03"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "ftp"
  - "hp"
  - "printers"
  - "sysadmin"
  - "technology"
coverImage: "hpprnftp_cover.jpg"
---

Look, I know, you just read the post title and thought _"What in the blue hell was James on when he was writing this blog post"_ but please, hear me out.

While working on a remote networked HP LaserJet printer I needed to reboot it so it would receive a new DHCP IP address. Naturally I trawled through the HP web gui to find a reboot option but weirdly enough there is none to be found. Telnetting the printer also was fruitless.

I really wanted to get this printer sorted and time was running out. After doing a bit of Googling I found out you can reboot a HP printer via FTP. Weird but okay, I'll take anything at this point.

So let's breakdown how to reboot a HP printer via FTP.

1. Prepare a text file called ```reboot.txt``` and file it with the following:
    
    ```plain
    %-12345X@PJL COMMENT
    @PJL DMINFO ASCIIHEX="040006020501010301040105"
    %-12345X
    ```
    
2. Drop to a Command Prompt and connect to the printer via FTP with the following:
    
    ```plain
    ftp x.x.x.x
    ```
    
    Once connected, you will be prompted for a **Username** followed by a **Password**. Leave both blank and hit the **ENTER** key to continue.
3. Now that you are authenticated enter the following to send the reboot file to the printer:
    
    ```plain
    put x:\pathtofile\reboot.txt
    ```
    
    Ensure that you replace _x:\pathtofile\\_ with the full path to where you stored the ```reboot.txt``` that you made in step 1.

That's it, once the file has uploaded to the printer it will reboot like some kind of FTP magic!
