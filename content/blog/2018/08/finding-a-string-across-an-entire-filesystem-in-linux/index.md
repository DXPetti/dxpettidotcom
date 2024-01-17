---
title: "Finding a string across an entire filesystem in Linux"
date: "2018-08-13"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "grep"
  - "linux"
  - "raspberry-pi"
  - "raspbian"
  - "technology"
coverImage: "grep_cover.jpg"
---

After coming back from a extra long holiday and noting that I hadn't received any [monitor tweets in a while from my Raspberry Pi]({{< ref "blog/2016/07/predictable-failure-tweeting-with-python-part-1" >}}), I thought it was a good time to login and make sure everything was running as before. Sadly, it wasn't even getting an address on my home network. Once I had nutted out a few issues (more to come in future posts), I saw there was a script that wasn't correctly running at startup.

![](images/grep1.jpg)

This _'gettext.sh'_ script was not familiar to me and had me concerned. My first thoughts ran to my ```rc.local``` file in **/etc/** but that the contents was unchanged. Next I inspected all my [cron tabs]({{< ref "blog/2018/02/keeping-your-pi-hole-container-fresh-with-cron" >}}) but once again, nothing out of the ordinary. How would I track down this mysterious script file?

## Enter grep

For those of you new to Linux distributions; grep is a fabulous tool that allows you to search the contents of data for strings in any variety. Very powerful when you combine it with other tools such as ```cat``` to output the contents of files like for example, log files.

Usage is simple;

```bash
grep stringthatIamlookingforward /location/to/search/in
```

So how would I find this elusive script file when it could be anywhere on the file system of my Raspberry Pi? First, because I want to search the entire file system, we should navigate to the root of said file system;

```bash
cd /
```

Now we can use grep to find the bugger!

```bash
grep gettext.sh
```

But hang on, nothing is happening. That is because grep is a processor of input and thus expects to be fed. So what happens if we explicitly specify the root of the file system, _'/'_ ? Well, you'll be reminded that _'/'_ is a directory and not a file. So how do we feed grep the entire file system? Well, tell it to search recursively with the -r switch like so;

```bash
grep -r gettext.sh
```

But I must warn you, if you do that, the majority of your result will be

![](images/grep2.jpg)

That is far too much noise when finding a needle in the digital haystack. Sure, we could run the search under ```sudo``` but as the script failure appears at logon it surely must exist in userspace land. Lets take a lesson from all the way back in the early days of [scripting cryptocurrency miners to start on boot]({{< ref "blog/2015/01/mining-bitcoins-with-raspberry-pi-part-2" >}}) and send the _'standard error output'_ ala **2>** out to the blackhole of **/dev/null**. Thus, the final one liner will look like this;

```bash
grep -r gettext.sh 2> /dev/null
```

![](images/grep3.jpg)

Now these are results I can work with. Off to chase this rabbit into Wonderland...
