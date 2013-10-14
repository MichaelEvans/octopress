---
layout: post
title: "Using a ViewPager in a ScrollView"
date: 2013-10-13 20:48
comments: true
categories: [Android, Tutorial, Code]
description: "ViewPager in a ScrollView"
keywords: "Android, ScrollView, ViewPager"
---

For a new app that I'm working on, I wanted to use a ViewPager inside a ScrollView in order to make a simple image carousel. The only problem is, ScrollViews don't seem to play nicely with horizontally swiping Views. As soon as the user moves their finger even slightly up or down, the ViewPager page change is cancelled, and the page snaps back to the current item. This can lead to an extremely frustrating user experience, because it is difficult for the user to understand why the views won't swipe.

<!-- more -->

A popular solution that I've seen online is to use the ViewPager's `getParent().requestDisallowInterceptTouchEvent(true);` method to prevent the ScrollView from scrolling while the ViewPager is being touched or scrolled. The solution that I've been using is a bit like this, but it provides a little bit of a "drag threshold", which is a bit forgiving in the event that the user is trying to drag the ScrollView, but happens to have their finger in the ViewPager. The code I used is as follows:


``` java PagerTouchListener.java
mPager.setOnTouchListener(new View.OnTouchListener() { 
 
    int dragthreshold = 30;
    int downX;
    int downY;

    @Override 
    public boolean onTouch(View v, MotionEvent event) {

        switch (event.getAction()) {
        case MotionEvent.ACTION_DOWN:
            downX = (int) event.getRawX();
            downY = (int) event.getRawY();
            break; 
        case MotionEvent.ACTION_MOVE:
            int distanceX = Math.abs((int) event.getRawX() - downX);
            int distanceY = Math.abs((int) event.getRawY() - downY);

            if (distanceY > distanceX && distanceY > dragthreshold) {
                mPager.getParent().requestDisallowInterceptTouchEvent(false); 
                mScrollView.getParent().requestDisallowInterceptTouchEvent(true); 
            } else if (distanceX > distanceY && distanceX > dragthreshold) {
                mPager.getParent().requestDisallowInterceptTouchEvent(true); 
                mScrollView.getParent().requestDisallowInterceptTouchEvent(false); 
            } 
            break; 
        case MotionEvent.ACTION_UP:
            mScrollView.getParent().requestDisallowInterceptTouchEvent(false); 
            mPager.getParent().requestDisallowInterceptTouchEvent(false); 
            break; 
        } 
        return false; 
    } 
}); 
```

I found this very useful solution on [StackOverflow](http://stackoverflow.com/questions/8381697/viewpager-inside-a-scrollview-does-not-scroll-correclty/16224484#16224484), among many other solutions that did work (although not as nicely as this one).