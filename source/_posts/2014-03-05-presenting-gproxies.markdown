---
layout: post
title: "Presenting GProxies"
date: 2014-03-05 00:12
comments: true
categories: [Linux, Gtk+, Vala]
---

Lately, I've been working only from my laptop, and since I'm moving a lot usually
I find myself in situations where I need to change networks details to properly connect
to Internet. Now, [GNOME][1] and [NetworkManager][2] network configuration system
does a decent job at remembering your network settings across different network.

The thing about this, it's that a network profile as defined in **System Settings** does not
keep your proxy configuration for that profile. So I took the task and wrote a small
tool for keeping different proxy configurations and allowing easy switching between them.

The tool is called **GProxies**. It's [hosted on Github][3]. It's built using latest
[Gtk+][4], **GLib** and **Vala**. I tried to use the minimal amount of dependencies
so it won't have that much noise.

**GProxies** is designed with a very simple plugins system: One main *default plugin*,
and some other living inside `${XDG_DATA_DIR}/gproxies`. The *default plugin* change
GNOME proxy system settings, and with the rest of the plugins you can change any other
configuration you might want using the activated proxy details. For instance: I've built
a plugin for updating also my git's `http.proxy` config when activating a proxy configuration.

Basically, you can write a plugin to do almost whatever you want. A plugin is defined by:

* a folder in `${XDG_DATA_DIR}/gproxies` with two or more files in it
* a `plugin.ini` key-file
* an executable script/application which receives proxy details as calling arguments

In this form you can make plugins for changing the proxy on whatever applications
you want. I've made another repository in Github at [gproxies-plugin][5] to keep
contain some of the plugins I've made for me. I'm planning to develop one for
updating the XChat proxy settings.

Right now the tool is in a working state. I have some other improvements in mind.
Those consist of:

* Add some interface to properly *enable/disable* plugins
* Add the ability of installs plugins from zipped files or remote sources
* Add a direct-connection (no proxy enabled) state

Finally, I'd like to say how much I enjoyed coding a small application for the GNOME
desktop. Gtk+ toolkit is perfect for this kind of task, with widgets which helps
enforcing GNOME HIG making an application fits properly in the desktop environment.
Also, Vala as a language it's really powerful and composite widget templates from Gtk+
really speed up the development process. The code of **GProxies** is pretty simple,
and I guess would make a nice reading for GNOME developer beginners.

Below, I present some screenshots. I hope this will be useful to some of you.

{{ 'gproxies_march_2014' | image_list }}

[1]: https://www.gnome.org
[2]: http://www.gnome.org/projects/NetworkManager
[3]: https://github.com/erick2red/gproxies
[4]: http://www.gtk.org
[5]: https://github.com/erick2red/gproxies-plugins