---
layout: post
title: "Software updates"
date: 2012-11-30 23:40
comments: true
categories: [Gnome, Calendar]
---
It's been a very long time since I posted anything.
You might not know, I live in [Santiago de Cuba][1], Cuba, one of the cities trashed by the [hurricane
Sandy][2]. That left us a bunch of loses and lack of communication for weeks. We went
without power for at least 14 days, so you can imagine why I was not posting updates. Some time has
passed and the city is growing back from its ashes.

I've been working on [**Calendar**][3], trying to get some minimal functioning
application, to get something out for you to use, and then I can continue
improving the rest. For now, I've focused on enable a fully working the "Event Details"
dialog. The thing is kinda clunky yet, but it works. It needs more polish
and heavy testing, though.

I also went and made a year-view implementation, which is almost fully
functional. This one was easy, was almost copy/paste from the old month-view
implementation.
The month-view also received a full face-lift. The previous version had some
super-rough edges when months didn't fit in a 35 cells grid. That has been
fixed, our designers pulled some rabbits out of their hats. The new design,
which is already implemented suggest a scrolling
animation, but I want to be sure it works before going into it.

I'm trying to get **Calendar** in beta
shape for 3.8 release. I'm still need to implement three views: list-view,
day-view and week-view, and the search mechanism, with the results showing.
For the week-view I have an implementation which have scrolling issues,
I have to take care of.

Some minor mojo I don't want to pass unnoticed:

* Squared buttons in the toolbar, thxs to Debarshy Ray for the suggestion and Cosimo's libgd for the hack.
* We added a proper use of `g_clear_object`
* Simple keyboard shortcuts, for **Quit** and change amount views.

Now some screenshots

{{ 'calendar_dec_2012' | image_list }}

[1]: http://en.wikipedia.org/wiki/Santiago_de_Cuba
[2]: http://en.wikipedia.org/wiki/Hurricane_Sandy
[3]: https://live.gnome.org/Design/Apps/Calendar
