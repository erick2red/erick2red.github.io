---
layout: post
title: "June update on Calendar"
date: 2013-06-25 21:19
comments: true
categories: [Gnome, Calendar]
---
I've made a commitment with myself of posting updates of the stuff I'm working
once a week. This one is the first, and I expected to be out by Monday, but an
[octopress][1] update and a plugin installation for handling image galleries
kept me from doing it.

By now, I'm a little tired of writing, therefore I'll be quick about it.

A few weeks ago I restarted the work on [Calendar][2] with the hope of adding a
bunch of new features in **Gtk+** to the project. I started by creating a
new branch, since I was sure the build would break a lot times in between, and
didn't want to spoil **master**

I'd set for myself a bunch of points I want to accomplish by rewriting the UI and
some of them has been done, some others, not yet. That's the main reason why I keep
the branch still around. As soon as I feel the work is done, I'll come back to work
on **master**

I wanted to:

1. Ditch **clutter** dependency. Use only Gtk+ widgets.
2. Include [GtkSearchBar][3], [GtkStack][4] and some of the latest Gnome UI guidelines
3. Use [GtkBuilder][5]'s ui files as much as I can
4. Rework week-view to use [GtkOverlay][6] instead of my own solution

So far 1, 2 and 3 has been accomplished. Point number 4 is going well since I manage to
craft a day-view with the same requirements of week-view that does not need to
use **GtkOverlay**.

Some of the work done is showcased below

{{ 'calendar_june_2013' | image_list }}

[1]: https://github.com/imathis/octopress
[2]: https://live.gnome.org/Design/Apps/Calendar
[3]: https://developer.gnome.org/gtk3/3.9/GtkSearchBar.html
[4]: https://developer.gnome.org/gtk3/3.9/GtkStack.html
[5]: https://developer.gnome.org/gtk3/3.9/GtkBuilder.html
[6]: https://developer.gnome.org/gtk3/3.9/GtkOverlay.html
