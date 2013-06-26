---
date: '2011-06-28 13:13:41'
layout: post
slug: developing-for-gtkglib
status: publish
title: Developing for Gtk/Glib
wordpress_id: '49'
categories:
- Development
tags:
- GLib/Gio
- Gnome Contacts
- Gtk
- Linux
---

Hi everyone. It's been a while since I blogged something. Been busy day-to-day work.

Yesterday something started to bug me, a new project like, a new toy application. After some modeling/sketching on a page I decided to start writing some demo code.

It took me a while to notice that I'm far from setting a GObject development environment fast. I'm using [Eclipse IDE for C/C++ Linux Developers](http://www.eclipse.org/downloads/packages/eclipse-ide-cc-linux-developers-includes-incubating-components/indigor), and valgrind and the usual tools in my Archlinux. There's, by the way, a package group in pacman called base-devel, I always had installed first hand.

Anyway I'll talk about the main three issues I found in my way to set up start and star coding some mockups and early designs of my toy app.

**Autotools packaging**

I'm not the guy who lined up with cmake cause autotools is old, and should be improved, although I think it needs some improvements [1]. The main issue with autotools is that it is hard to learn, the existent documentation is little and somewhat cryptic, and that makes the learning curve steep. After read some things on the web, and been this not first time I stand before the issues, I did what I think everyone should, I made my own sample-autootols project which keeps a minimal configuration of what I need to develop a Gtk application. I included tips from [this post](http://blogs.gnome.org/jjardon/2010/01/25/modernize-your-autotools-build-system/) about improving and modernizing the autotools configuration, and some others about using [xz compression instead of gz](http://mail.gnome.org/archives/desktop-devel-list/2011-May/msg00599.html). The project is in [github](https://github.com/erick2red/autotools-sample), and I'm planning to improve it with several configurations for several kinds of builds. Anyway, that for point one, I will keep my template at hand, but I think for new developers, and for the good of the platform, Gnome should make something like Ubuntu Quickly. Note: I've tried to use anjuta, really tried, but it doesn't feel right or stable, and this is not for the developers to take it personal, it just my experience with it had been bad.

**GObject based C class**

This is so simple is really a shame it had taken me so long to pass this point. I needed some GObject based class, to handle some  internals structures of my application, and then I found myself copying parts of devhelp tutorials to headers and sources files to build the class, this code is all templates, is the same over and over for every class I had and have to make. I just want some script that make me the files after I provide the name of the class I want. If I had the time to learn howto make Eclipse plugins I'll make the Eclipse Template for those files header and source file for  a GObject based class. I tried vala before and I know it does, one possible solution is for vala to print the translation into some pretty formatted code. If I can't find any solution soon, I'll make some awk script that output the files with the required boilerplate needed.

**Valgrind debugging**

Finally, the third point is the fair beginner one, I have to say this pretty much my first time doing C development of Gtk/Glib applications. Until now I've used some vala, and some JavaScript, and some C++, but C, nope, so I think this is mainly why I found this obstacles in my path. After having some windows running and some buttons doings it's work I felt it was time to profile the memory usage I was using. I used valgrind since the Eclipse bring with support for it included, and I notice I need a suppression file for Gtk/Glib _possible memory lost_, which isn't real. Digged a little on the web, actually digged nothing this was the first results that pops out: [Valgrind at live.gnome.org](https://live.gnome.org/Valgrind), and I went after the suppression file, just to learn that the file was missing or something else (I'm getting some internal error, which I now realize I have to tell the maintainers of the wiki). After that I decided I have to do it by myself. I think that the gnome developers should have a file like that somewhere, but I'm to shy to ask for it, so I'll take the time to do one for me, someday.

Again I think that, in order to promote the platform, is necessary to publish things like this, The new look of [developer.gnome.org](http://developer.gnome.org/) and the samples in there helps to that purpose, but still feel more work in that direction is missing.

[1] Autotools needs to handle parallel configure, since parallel _make_ is handled by the _-j_ flag.
