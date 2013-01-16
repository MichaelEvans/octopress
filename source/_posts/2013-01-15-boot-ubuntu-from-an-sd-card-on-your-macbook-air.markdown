---
layout: post
title: "Boot Ubuntu from an SD Card on your Macbook Air (or Pro)"
date: 2013-01-15 22:26
comments: true
categories: Guides
description: "Booting Linux off an SD Card using a Macbook Pro or Macbook Air"
keywords: "Linux, Apple, Tutorial"
---

Recently I was trying to find a way to run Ubuntu off an SD card, so that I could use Linux on my Macbook Air (I'd have dual booted, but my Air has the 128GB SSD, which doesn't leave much space after partitioning). Scouring the web, there's not that many resources indicating how, or if this is even possible. Fortunately for you, dear reader, it is! 

Like I said, I'm using a Macbook Air, so there's two steps to this process: creating a bootable flash drive with the Ubuntu image, and the actual installation. 

<!-- more -->

###Creating a bootable install image

This is pretty standard stuff, if you're reading this, it's likely you already know it. Heck, if you have an external DVD drive, you can skip this all together. 

1. Download the image from [Ubuntu](http://www.ubuntu.com/)
2. Hit up your terminal and use the following command to convert that image `hdiutil convert -format UDRW -o ~/path/to/target.img ~/path/to/ubuntu.iso`
3. More terminal goodness: `diskutil list` to find which disk is your flash drive
4. `diskutil unmountDisk /dev/diskN` where N is the disk number you found in step 3.
5. `sudo dd if=/path/to/converted.iso of=/dev/diskN bs=1m` to copy that image over to the flash drive.
6. `diskutil eject /dev/diskN` to EJECT!


### Setup the SD Card

Now we're ready to install Ubuntu on the SD card (SD card must be at least 8 Gigs). I used a Class 10 card, and performance was…not terrible. I can't speak for any of the slower cards.

1. Download rEFIt from the [rEFIt website](http://refit.sourceforge.net/). You should grab the .dmg version)
2. Open up Disk Utility (in /Applications/Utilities) and create a partition of 50 MB formatted "Mac OS Extended Journaled"
3. Fill up the rest of the card with a partition of MSDOS-FAT. 
4. Before writing the Partition Table, hit the options button and select "GUID Partition Table"
5. Copy the 'efi' directory onto the root of the 50MB partition created in step 2.
6. Get back to that trusty terminal `cd /Volumes/partition1/efi/refit` (where 'partition1' is the name of that same parition) followed by `./enable.sh`
7. Reboot your machine, and press and hold the option key to select a boot device. You should see ones for your main HD, the SD card, and the Live CD image. Select the Live CD image, and let's get installing.
8. During installation, you'll want to select the advanced partitionint setup. Select the /dev/sdb3 device (Should match the size of the SD card) for formatting and give them the mount point "/". Device for boot loader installation must be also /dev/sdb3 (not /dev/sdb!). Ubuntu is going to give you a warning about no swap partition. That's alright, you don't need it.
9. This is going to take a while. Seriously, it's not gonna be fast. Go get a coffee.
10. Reboot when the install is complete, and hold option again to select that SD card. *Ta-da*!

#####Bonus Step:
After you remove the SD card and try to boot normally, you'll likely hit this error "grub error: no such device"

You can fix this by booting into OS X and selecting your hard drive as the Startup Disk, which can be found in the System Preferences.


**Did you find this useful? Feel free to comment below, or follow me on [Twitter](http://www.twitter.com/m_evans10) and let me know if you have any questions, or would like to suggest things for me to talk in the future.**