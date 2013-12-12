---
layout: post
title: "ColorArt: a library to do iTunes 11-style color matching for Android"
date: 2013-12-12 00:02:34 -0500
comments: true
categories: [Android, Development, Code, Github]
description: "ColorArt: a library to do iTunes 11-style color matching for Android"
keywords: "Android, Development, iTunes, Library, Github"
---

Last week I open sourced an Android library called [ColorArt](https://github.com/MichaelEvans/ColorArt), which is a port of the popular OS X library written by the guys over at [Panic](http://www.panic.com/blog/itunes-11-and-colors/). It allows you to use a source image to create a themed image/text display, very similar to the effect which is done in iTunes 11. 

{% img center /images/2013/12/12/itunes-11.png 600 600 iTunes Example %}

<!-- more -->

###Using the Library

ColorArt is pretty easy to use, and can add a cool effect to any app. In order to add it to your project, add the dependency to your `build.gradle` file:

```
compile 'org.michaelevans.colorart:library:0.0.1'﻿
```

The next thing you need is to take a Bitmap that you want to use, and pass it to a ColorArt object:

```
Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.drawable.album);
ColorArt colorArt = new ColorArt(bitmap);
```

After that, it's as simple as calling some getter methods in order to get the colors that make up the image: 

```
colorArt.getBackgroundColor()
colorArt.getPrimaryColor()
colorArt.getSecondaryColor()
colorArt.getDetailColor()
```

Voilà! Now you can make cool themed image/text display using Bitmaps as part of your application.

{% img center /images/2013/12/12/sample.png 300 300 Sample %}

###Bonus: FadingImageView

I also added a class that extends ImageView, which will allow you to add a subtle gradient from the image, to the background color, similar to what you see in the iTunes display. It's also extremely easy to use:

1. Replace the ImageView that you are presently using with a `FadingImageView`.
2. Call 
```
mFadingImageView.setBackgroundColor(colorArt.getBackgroundColor(), FadingImageView.FadeSide.LEFT);
```
to set the background color to fade to, and which side gets the gradient.

That's it!

You can also toggle that gradient with a call to `mImageView.setFadeEnabled(true/false);`.

---

Are you using this library in your app? Please let me know so I can add a link to it on the [GitHub project page](https://github.com/MichaelEvans/ColorArt)! Also, feel free to send pull requests!

