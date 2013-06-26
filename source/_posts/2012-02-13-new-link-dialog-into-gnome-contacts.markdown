---
date: '2012-02-13 23:45:24'
layout: post
slug: new-link-dialog-into-gnome-contacts
status: publish
title: New Link Dialog into gnome-contacts
wordpress_id: '78'
categories:
- Gnome Contacts
tags:
- Gtk
- Updates
- Vala
---

Well, it seems I'm fully contributing to gnome-contacts now.

... that was the start of this blog post a few weeks ago, the work got me in the middle and the post got stuck at draft stage til today. So

In these previous weeks, I got the new link dialog mockups made by [Allan Day](http://afaikblog.wordpress.com) into the code. After some hacking to the undo part well. I uploaded the patches into bugzilla for review. Here's the mandatory screenshot

[![](http://erick2red.files.wordpress.com/2012/02/link-to-other-contact.png?w=300)](http://erick2red.files.wordpress.com/2012/02/link-to-other-contact.png)

After that I went into getting crop support for setting an avatar for a contact. Here I had to came up with some design, since Alex and Allan have not agreed anything on these. I took the crop widget from gnome-control-center code, and tweaked the vapi file to load the widget. Now when you use a picture with more than 128 x 128 pixels as avatar the crop widget come up to allow you to select what you want of that pics. The crop widget only allow for square crop. Here the screenshot is this one:

[![](http://erick2red.files.wordpress.com/2012/02/contacts-crop.png?w=225)](http://erick2red.files.wordpress.com/2012/02/contacts-crop.png)

Right, now, I'm kinda frozen. There's still a lot of stuff to be done. We'll set to it soon.
