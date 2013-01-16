---
layout: post
title: "A Guide to Unlocking and Rooting your Android Phone"
date: 2013-01-09 10:45
comments: true
categories: [Android, One V, Modding]
description: "A guide to unlocking and rooting an HTC One V"
keywords: "Android, rooting, unlocking, HTC One V, One V, Tutorial"
---

Something that is very popular in the Android world these days is rooting and installing custom ROMs on your phone. A problem that I've found however, is that information regarding the process, and the terminology involved can be quite confusing for someone getting started, and as a result might discourage them. I decided to write up a series of blog posts discussing various parts of this process, so that it might help someone who wants to get involved, but doesn't know how. The phone that I'll be using for these tutorials is an [HTC One V](http://amzn.to/VAQwcM), which can be used on Virgin Mobile. It's a fairly modern phone, comes with a 1GHz single-core CPU, 512MB RAM, and a nice 3.7 inch LCD. This process will almost identical for other HTC phones, and very similar for many other Android devices. 

Disclaimer: This may void your warranty, I take no responsibility, etc.

<!--more-->

## Part One: Unlocking your One V

The first part of the process is called "Rooting" your device. Rooting basically means obtaining admin access on your device, which allows you to do all sorts of fun things down the line. In order to root your device however, there are a few steps you'll have to take. The first thing you'll need to do is unlock the bootloader. The bootloader is the software that loads the operating system of your phone. Normally this is locked to only start the software that ships on the device, but we are going to unlock it so that we may install custom software. 

1. Go to [HTC Dev](http://www.htcdev.com/) and create yourself an account.
2. Download [these](https://www.box.com/s/kuouqnnsjke4byd2u38n) Fastboot Files
3. Make a new folder on your C: or home directory called fastboot.
4. Extract the zip downloaded in step 2 to the new folder
5. Go [here](http://www.htcdev.com/bootloader/) and select "All Other Supported Models" and then "Begin Unlock bootloader"
6. Check both checkboxes, and then proceed.
8. Power off your phone by holding the power button down for 5-7 seconds (the three keys will flash a few times then it will power off)
9. Hold the volume down button and power the device on, the device will boot to a white screen called "HBOOT"
10. Select "Fastboot" with volume up or down then press the power button once (you'll be in fastboot mode)
11. Plug your phone into your computer and open a Command Prompt.
12. cd to your folder where we put fastboot files (i.e c:\fastboot)
13. type this into your command prompt: `fastboot oem get_identifier_token`
14. Select & copy the text as per step 9 on the HTC Dev website
15. Paste your identifier token into the box at the bottom and submit
	You will then receive an email from HTCDev with a link to the second 	part of the instructions and your "Unlock_code.bin" file attached.
16. Download the "Unlock_code.bin" attachment from the email and save it to 	the same folder that we extracted fastboot files.
17. Type this into your command prompt window: `fastboot flash unlocktoken Unlock_code.bin`
18. The display will change on your phone, and ask you to confirm the unlock. press volume up to accept and power to confirm.

**Congratulations**! You have successfully unlocked the bootloader.

##Part Two: Rooting the One V

The next step in this process, is actually obtaining root access. In order to do that, we will be installing an application called SuperSU, which allows you to run applications on your phone with administrative priveledges (also known as superuser or root). 

1. Download [r1-primo-cdma-superboot.zip](https://www.box.com/s/zli0tchzdbs6dtwgv7px) to your computer and extract in any directory.
2. Power off your phone by holding the power button down for 5-7 seconds (the three keys will flash a few times then it will power off)
3. Hold volume down button while powering on, the device will boot to HBOOT (like we did before)
4. Select Fastboot with volume up or down then press power once
5. Connect your phone to PC
6. Double click ‘install-superboot-windows.bat’ (or mac or linux, depending on your platform)

**Excellent!** You now have root access on your phone. When you start up Android, you'll see a new application in your launcher. It's called SuperSU, and will ask you to allow/deny applications root access if they request it. This is useful for installing things like tethering applications, or modifying system files.

##Part Three: Installing Custom Recovery

The last thing we are going to do as part of this guide is install a custom recovery. A recovery is a mode of the phone that allows you to backup/restore your device, or modify device files, like installing new software. This will be important if you'd like to backup your device before wiping it to install custom software. In order to install ClockworkMod Recovery (which is probably the most popular recovery you'll find):

1. Download ClockworkMod Recovery [here](https://www.box.com/s/vj181v9mmi5tghm1za4b).
2. Power off your phone by holding the power button down for 5-7 seconds (the three keys will flash a few times then it will power off)
3. Hold the volume down button while powering on, the device will boot to HBOOT (You should be getting the hang of this by now)
4. Select Fastboot with volume up or down then press power once
5. Connect your phone to PC
6. Type the following in to your command prompt: `adb flash recovery cwmrecovery.img`
7. Restart your phone, and hold the volume down button. This time, select "recovery" and hit the power button. 

You're now in recovery mode! While we're here, let's make a backup. 

1. Scroll through the list of options 2 times using volume up or volume down. (Without doing this, you won't be able to select an option)
7. Use the power button to select "Backup and restore"
8. Select "Backup"
3. The backup process is now running. It will be a few minutes to complete.
4. Once complete, let's make sure the backup is successful. 
5. Once again, select "Backup and restore"
6. Choose "Restore". You will see your backup files, e.g: 2013-01-12.10.13.04
7. Pick "Go Back"
8. Select "Reboot system"


It's probably best to copy that backup folder off your SD card and onto your computer in the event something happens to your SD card.

**Awesome**! You're now all set to do all sorts of things, like installing custom roms. Be sure to check back for the next part of this series, where we will go over that.