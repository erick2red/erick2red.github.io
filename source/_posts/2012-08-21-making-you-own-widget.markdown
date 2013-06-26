---
layout: post
title: "Making you own widget"
date: 2012-08-21 13:53
comments: true
categories: [Code, Gtk+]
---

A few days ago, I was in [gtk+ irc channel][1] and there was someone making the
kind of question I've done myself like a hundred times. Thankfully the guys
in the channel hasn't got tired of me, yet. They are pretty helpful once they
realize you've done your homework.

Once you start hacking around with Gtk+, the next thing you'll want to do
is to make your own widgets. And for that there's little to none documentation,
guides or tutorial. There's a huge amount of knowledge in Gtk+ source itself, but most
people doesn't like to read code, or doesn't know how to. They should, [though][2]

Here I'll try to show, from the newbie point, how Gtk+ draw things,
and what's the guidelines to accomplish certain tasks when making
widgets for Gtk+. This is, as well, a reminder for myself, since most times I
need to go into IRC to ask the same things all over again.

I will split the post in three main sections which corresponds to the important
parts in a widget lifetime.

#### Size handling ####

One of the thing you need when you're making widgets is knowing the
dimensions everything will have inside. Size requisition and allocation is the
step where this happens.

Explanations of this process appears here in [GtkWidget's documentation][3].
It includes most of the size requisition process and geometry management.

Gtk+ widgets are arranged in a hierarchy where container widgets pack others
and handle its size and allocation. The widget containing yours will ask the
natural and minimal size you want to have, and after making some internal
calculations will assign your widget a size and allocation. The first
part is called *size requisition* and the second *size allocation*. The
virtual methods you need to implement here are:
 
* *For size requisition*
  * [`gtk_widget_get_preferred_width`][4]
  * [`gtk_widget_get_preferred_width_for_height`][5]
  * [`gtk_widget_get_preferred_height`][6]
  * [`gtk_widget_get_preferred_height_for_width`][8]

* *For size allocation*:
  * [`gtk_widget_size_allocate`][8]

Now, what to do on each one of these methods ?, Calculate what size you want for
the widget, and return it. Let's say you have a widget which have a text saying
**"Hello"** inside, then, you need to ask [Pango][9] to calculate the width and
height for you, add the margins and padding (if you want to take those into
account) and return the size accordingly.

What's width-for-height and height-for-width methods for ?, Widget have by
definition one request-mode, its width depends on its height or the other way
around. This is totally decided by you, accordingly to the type of widget you're
creating. The purpose of these methods is to retrieve these measures, the width
of a widget given a specified height, and viceversa. About this part,
read carefully the docs above and go asking in the IRC for more help if stuck.

One thing to keep in mind is that the allocated size to your widget, could be
not the same you requested in the `get_preferred` methods, so the widget should
work with the given allocation instead of any other measure.

*Rule of thumb*: Implement the four methods of size requisition almost always,
and the size-allocate one if needed.

#### Drawing stuff ####

This is the core of the matter, but there's no much to tell in here. Use
[cairo][10] to draw, the API is pretty simple. Use cairo primitives to draw
inside the implementation of `draw` virtual method. There, is given a context
which is clipped from one of the `GdkWindow` of the widget, you should draw
in that context. Other than this, here are some remarks:

* How to draw an icon from the theme into the `cairo_context_t` ?. Obtain a
  `GdkPixbuf` from the icon with [`gtk_icon_theme_load_icon`][11] or
  [`gtk_icon_info_load_symbolic`][12] depending on the icon, and draw it to the
  context with [`gdk_cairo_set_source_pixbuf`][20]

* How do I get my lines subpixel thin ?. Turns out that cairo can draw lines
  thinner than one pixel, but for that to happen you need to place the cairo
  starting point in the middle of the pixel, so the drawing can be made, and look
  just like you want.

      /* a vertical 0.3 pixel line */
      cairo_set_line_width (cr, 0.3);
      cairo_move_to (cr, 10, 10.4);
      cairo_rel_line_to (cr, 0, 10);
      cairo_stroke (cr);


Finally, use whenever you need [`gtk_render_background`][13],
[`gtk_render_frame`][14] family and friends over any alternative drawing you
could think of doing. These functions will get colors and border size
and radius from the CSS theming engine, therefore you're spared a bunch of work.

#### Processing events ####

How to receive and handle mouse, touch and button events. My first attempt
to catching and handling event was to implement virtual method `button_press`
and `button_release`, that didn't worked. Not that easily.

For catching input events in your custom widget, you will need a `GdkWindow`
suited for those purposes. The preferred idiom here is:

* In your `realize` method, create a `GdkWindow` with [`gdk_window_new`][15] and
  set its type to `GDK_WINDOW_CHILD`, this would assure to catch the input events if
  they occur inside you window. After created the window, call
  [`gdk_window_show`][16] to show it. You will use
  [`gdk_window_set_user_data`][21] to relate the window with your widget.
* In your `size_allocate` method, you need to place the window where it should,
  by using [`gdk_window_move_resize`][17].
* Now, you can implement `button_press` and the likes of them to handle your
  events.

There are virtual methods for handling mouse button presses, and a releases,
but no for high-level actions as click and double-click, you need to detect those
by detecting sequences of presses and releases. Nonetheless, for double-click the
`GdkEventButton` will have the type field set to `GDK_2BUTTON_PRESS` which will
certainly help. For key-press/release events, I have to say, I haven't
had nothing to do with those yet. So, that will be left to the next time.

#### Putting things together ####

I won't paste the code entirely, I'll just describe the order in which the above
mentioned steps will most likely occur.

Usually, as you must have guessed by now, the first thing your widget will
receive is a request for its preferred size using
`gtk_widget_get_preferred_width` and friends. After that, the
parent will follow to allocate the size, there your implementation of
`size_allocate` will run, and then, the painting will happen.
About the `realize` part, it will happens almost anytime,
the only thing you can be sure of is: it will happens before your widget had to
paint itself. One thing worth mentioning is that a widget could be allocated a
bunch of times before it ever gets to the painting part.

About a full example of code, one the simplest widgets for showing all
of this is `GtkButton`. You can read the source of in the Gtk+ tree. Here's
the header: [`gtkbutton.h`][18] and the source [`gtkbutton.c`][19]. As I said
above, reaching into Gtk+ internals is the best way to learn to use the toolkit
and the framework.

#### Conclusion ####

Obviously this a pretty simple post about some of the
more common tasks you'll meet with when making a custom widget. There are
several more complex widgets which are far from this, but those are left for
later.

Hope it helps, and if you find any mistake, please contact me immediately so I can
fix it.

[1]: irc://irc.gnome.org/gtk+
[2]: http://www.codinghorror.com/blog/2012/04/learn-to-read-the-source-luke.html "Read the Source, Luke"
[3]: http://developer.gnome.org/gtk3/stable/GtkWidget.html#GtkWidget.description
[4]: http://developer.gnome.org/gtk3/stable/GtkWidget.html#gtk-widget-get-preferred-width
[5]: http://developer.gnome.org/gtk3/stable/GtkWidget.html#gtk-widget-get-preferred-width-for-height
[6]: http://developer.gnome.org/gtk3/stable/GtkWidget.html#gtk-widget-get-preferred-height
[7]: http://developer.gnome.org/gtk3/stable/GtkWidget.html#gtk-widget-get-preferred-height-for-width
[8]: http://developer.gnome.org/gtk3/stable/GtkWidget.html#gtk-widget-size-allocate
[9]: http://developer.gnome.org/platform-overview/stable/pango
[10]: http://developer.gnome.org/platform-overview/stable/cairo
[11]: http://developer.gnome.org/gtk/stable/GtkIconTheme.html#gtk-icon-theme-load-icon
[12]: http://developer.ginome.org/gtk/stable/GtkIconTheme.html#gtk-icon-info-load-symbolic
[13]: http://developer.gnome.org/gtk3/stable/GtkStyleContext.html#gtk-render-background
[14]: http://developer.gnome.org/gtk3/stable/GtkStyleContext.html#gtk-render-frame
[15]: http://developer.gnome.org/gdk/stable/gdk-Windows.html#gdk-window-new
[16]: http://developer.gnome.org/gdk/stable/gdk-Windows.html#gdk-window-show
[17]: http://developer.gnome.org/gdk/stable/gdk-Windows.html#gdk-window-move-resize
[18]: http://git.gnome.org/browse/gtk+/tree/gtk/gtkbutton.h
[19]: http://git.gnome.org/browse/gtk+/tree/gtk/gtkbutton.c
[20]: http://developer.gnome.org/gdk/stable/gdk-Cairo-Interaction.html#gdk-cairo-set-source-pixbuf
[21]: http://developer.gnome.org/gdk/stable/gdk-Windows.html#gdk-window-set-user-data
