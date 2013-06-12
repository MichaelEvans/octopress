---
layout: post
title: "Getting Mavericks' Compressed Memory Feature in Ubuntu"
date: 2013-06-12 00:16
comments: true
categories: [Ubuntu, Tutorial, Linux]
description: "Getting Mavericks' Compressed Memory Feature in Ubuntu"
keywords: "Linux, Ubuntu, Guide"
---

On Monday, Apple announced a new update for it's desktop operating system, which will be named OS X Mavericks. As part of the annoucement, they touched on a few performance related features, including a new power saving API, which was called App Nap, as well as a memory compression feature, which would allow compress the memory of programs that are not currently active. 

According to [Ars Technica](http://arstechnica.com/apple/2013/06/how-os-x-mavericks-works-its-power-saving-magic/), Apple is using a technique called Timer Coalescing to create App Nap. Apps that are not active will have their processes set to a lower priority, and therefore use less CPU/disk/etc, and thereby save power. This feature has been around on Linux for a long time, and apparently has been part of Windows as well.

I had not heard, however, of this compressed memory technique, although apparently it had been done many years ago by a program called "RamDoubler". After some quick Googling, it seems that this is available via a kernel module called "zram". 

<!-- more -->

Wanna try it out? If you're running Precise or newer (12.04 or later), you're in luck. Just apt-get zram with

```
sudo apt-get install zram-config
```

and you're all set.

To see the effects of zram, just run `free -m` and you'll notice an increase in memory/swap space. I haven't thoroughly tested to see the performance gains, but it's interesting to see that some of the new OS X features are already available elsewhere.

