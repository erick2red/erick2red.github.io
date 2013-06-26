---
layout: post
title: "Updates to gnome-calendar"
date: 2012-07-03 16:00
comments: true
categories: 
---

It's been long time since I published any stuff, errr. I've
been busy. So, to the point.

Calendar is moving forward, slower than I would like, but is moving. From the
last time, I've started the implementation of the event-view, and I say started
cause there's missing functionality yet, but the widgets part is almost done.
For now, it deletes your events, so be careful, that's the only button that 
really works. I've set in-place some code to enable notifications, but that will
have to wait a little yet. The toolbar has got some changes,
mostly added behavior to the change between views, and some internal
refactoring.

The most important stuff is: the screenshots you'll see below will
show you one thing, the event-view need some redesign to occupy all that empty
space. There's host/guest part still missing implementation, but I know it won't
fill all the space. On the other hand, I'll need some symbolic icon for the
reminders, to show in the views (monthly, daily, etc.). There's still the issue
of the navigation controls presentation, which I don't like been shown by
pressing <Ctrl>, but that will come eventually.

This work is pretty skeleton-like, there's a bunch of stuff that needs to be
retouched, so, any thing you find, feel free to reach me.

Now the shots:
The first one is the month-view, the only one working right now, and the others
are of the event-view.

{{ 'calendar_aug_2012' | image_list }}
