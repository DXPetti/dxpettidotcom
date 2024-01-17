---
title: "Mining Bitcoins with Raspberry Pi - Part 1"
date: "2015-01-05"
series: ["Mining Bitcoins with Raspberry Pi"]
series_order: 1
categories: 
  - "tech"
tags: 
  - "bitcoin"
  - "cgminer"
  - "cryptocurrency"
  - "debian"
  - "iot"
  - "linux"
  - "raspberry-pi"
  - "raspbian"
  - "technology"
coverImage: "rpi_mining1_cover.jpg"
---

Continuing with my [last post's]({{< ref "blog/2015/01/raspberry-pi-a" >}}) theme surrounding my recently received Raspberry Pi; after the initial configuration, I put it into action by mining Bitcoins with some Aussie USB ASIC miners I purchased of eBay for $36 (for two).

Now the Raspberry Pi itself would be rather useless as a mining device; with a humble 700MHz ARM CPU it is not going to break any records. Hell, even the more advanced CPUs like those found in desktops and servers have been found to be inadequate for mining Bitcoins as the difficulty continued to rise. Most miners made the jump from CPU to GPUs (or even multiple GPUs using technology such as Nvidia's SLI or ATI's CrossFire) and consequently went from around 150 MH/s to 500 MH/s. Not a bad jump for those who already had the setup.

But then came the ASIC...

ASIC or Application Specific Integrated Circuits are purpose built circuits. Do one thing and do it very well. In the case of Bitcoin, ASIC miners gave users in the GH/s range and doing so without it costing the bank to run.

Take for instance the Bitfury chipset based ASIC miners I currently use. Small, lightweight, USB Powered and push out around 2.2 GH/s. They make the CPU and GPU miners of yesteryear look positively wasteful. But that is the beauty of designing a system for a single purpose.

Now, enough of the history lesson and on to the mining.

Follow along to setup on your own Raspberry Pi, cgminer, the bitcoin mining application of choice to work along in tandem with USB mining ASIC hardware.

## Prepare dependencies

Before we install any software, let's ensure our repositories are up to date and all installed software up to date

```bash
apt-get update apt-get upgrade
```

Followed by installing the necessary libraries to work with cgminer + USB mining hardware

```bash
apt-get install libusb-1.0-0-dev libusb-1.0-0 libcurl4-openssl-dev libncurses5-dev libudev-dev autoconf libtool
```

## Clone, configure and deploy cgminer

Next, we will clone cgminer from github, configure it for our hardware and finally install it

```bash
git clone https://github.com/ckolivas/cgminer cd cgminer ./autogen.sh ./configure --enable-YOURASIC make sudo make install
```

Check the [ASIC Readme](https://github.com/ckolivas/cgminer/blob/master/ASIC-README "cgminer/ASIC-README at master · ckolivas/cgminer · GitHub") for the necessary command to replace ```YOURASIC``` and configure cgminer to work with your USB ASIC.

## Run cgminer

Alright, with cgminer installed, connect up your USB ASIC and open up cgminer

```bash
./cgminer
```

{{< alert "circle-info" >}}
If you receive error's later on regarding issues accessing your USB ASIC then you may need to run cgminer as someone with more permissions via
```sudo ./cgminer```
{{< /alert >}}

From here, cgminer will ask for a URL of a mining pool followed by credentials to authenticate to the pool.

Once you are up and running I recommend saving your working configuration to a .conf file. That way, next time you load up cgminer, you can begin mining straight away without reconfiguring.

When you have a configuration file to work with, use the below to make cgminer work off the file

```bash
./cgminer --config /path/to/config.conf
```

_Remember to replace ```/path/to/config.conf``` with the full path to your custom configuration file._

That's it for now, enjoy your bitcoin mining via the Raspberry Pi. [Part 2]({{< ref "/blog/2015/01/mining-bitcoins-with-raspberry-pi-part-2" >}}) will cover running cgminer as a background process (i.e. a service) with [Part 3]({{< ref "/blog/2015/01/mining-bitcoins-with-raspberry-pi-part-3" >}}) having that process auto-start with the Raspberry Pi.
