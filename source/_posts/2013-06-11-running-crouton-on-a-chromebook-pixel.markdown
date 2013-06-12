---
layout: post
title: "Running Crouton on a Chromebook Pixel"
date: 2013-05-23 21:22
comments: true
categories: [Google, Chromebook, Ubuntu, Linux, Guides]
description: "Running Crouton on a Chromebook Pixel"
keywords: "Google, Chromebook, Crouton, Linux, Ubuntu, Guide"
---

So a few weeks back at Google IO, Google handed out an awesome laptop to attendees, the [Chromebook Pixel](https://play.google.com/store/devices/details?id=chromebook_pixel_wifi). For those who aren't familiar with it, the Pixel is a laptop running Google's ChromeOS, with the highest resolution screen available on a laptop (2560x1700). Unfortunately for me (and owners of the other Chromebooks), there's a few things you can't do in Chrome, like writing software in the new Android Studio! 

In order to work around this limitation in ChromeOS, a developer at Google named David Schneider developed a set of scripts called [crouton](https://github.com/dnschneid/crouton). Crouton allows you to run Ubuntu inside a your ChromeOS system, so that you can easily swap back and forth between the operating systems. This is not limited to the Pixel, so if you're using another model Chromebook, even dating back to the CR-48, this guide should work for you.

<!-- more -->

###Installing Crouton

####Step One: Enable Developer Mode (This might vary slightly depending on your model)
*Warning: Switching into or out of developer mode will wipe all the user data on the ChromeOS machine.*

1. Shutdown your machine.
2. Press and hold esc + refresh, and push the power button to turn on your computer. 
3. Push Ctrl+D to be prompted to enable developer mode.
4. Hit enter, and watch the progress bar at the top of the screen as the system is wiped.
5. When the machine boots up again, either wait til it boots, or you can hit Ctrl+D again to start faster.

####Step Two: Install Crouton

1. Download crouton [here](http://goo.gl/fd3zc).
2. Open a shell (Ctrl+Alt+T, type shell and hit enter) and run `sudo sh -e ~/Downloads/crouton -t xfce,touch`.
3. Go get a drink, come back and answer some questions.
4. Done! Start up an XFCE session with `sudo startxfce4`.

####Step Three: Enjoy Ubuntu on your Chromebook!

To switch back and forth between ChromeOS and Crouton, swap with Ctrl+Alt+Back and the sequence Ctrl+Alt+Forward followed by Ctrl+Alt+Refresh.

Pro tip: Increase the font size under the display settings, so that the text is a little more readable!
