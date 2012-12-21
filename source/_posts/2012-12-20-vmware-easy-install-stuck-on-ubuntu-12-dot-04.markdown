---
layout: post
title: "VMware 'Easy Install' stuck on Ubuntu 12.04"
date: 2012-12-20 19:34
comments: true
categories: [Linux, VMware, Ubuntu 12.04]
description: "VMware Tools fix for Ubuntu 12.04"
keywords: "VMware Player, VMware Tools, Ubuntu 12.04"
---

I was trying to install Ubuntu 12.04 in VMware Player on my PC and ran into some trouble during the "Easy Install" process. The installation basically hung at the console after the installation was successful, with a message telling me that the VMware Tools were being installed, and that I could login and use the system while they were installing. Maybe 10 minutes later, there was no progress, so I restarted the VM. 

I decided to log in and start X manually, so I could at least mess around with it in the meanwhile. Without being able to resize the window/use the shared clipboard, I decided to try to install the tools manually. No dice here, since VMware won't let you install the tools while the Easy Install process is still running. Here's what you can do to fix it:

<!--more-->

{% codeblock lang:bash %}
sudo mv /etc/issue.backup /etc/issue
sudo mv /etc/rc.local.backup /etc/rc.local
sudo mv /opt/vmware-tools-installer/lightdm.conf /etc/init
{% endcodeblock %}

Then follow these few steps to stop the easy install process:

{% codeblock %}
Power off the virtual machine.
Go to VM > Settings
Select Floppy.
Select 'Use physical drive', and set the dropdown to 'Auto Detect'.
Power on the virtual machine and install VMware Tools. 
{% endcodeblock %}

Hope this helps!