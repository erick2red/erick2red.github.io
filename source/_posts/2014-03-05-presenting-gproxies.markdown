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
keep your proxy configuration for that profile. So I took the task to wrote a small
tool for keeping different proxy configurations and allowing easy switching between them.

The tool is called [GProxies][3]. It's built using latest
[Gtk+][4], **GLib** and **Vala**. I tried to use the minimal amount of dependencies
so it won't have that much noise.

**GProxies** is designed with a very simple plugins system. Plugins are meant for
updating other applications' configuration whenever you select a different proxy setting
from the interface. For instance: I made a plugin for updating my git's `http.proxy`
config automatically when selecting a different a proxy settings. There are two types
of plugins: one main *default plugin*, for setting GNOME system settings' proxy and
the others living inside `${XDG_DATA_DIR}/gproxies` designed to update other applications
configuration.

A plugin is defined by a folder named after the plugin in `${XDG_DATA_DIR}/gproxies`
with two or more files in it:

* a `plugin.ini` key-file. More details are explained [here][5].
* an executable script/application which receives proxy details as calling arguments

This way you can make plugins for changing the proxy in any applications
you want. I've made another [repository][6] with the plugins I've write for myself.

Right now the tool is in a working state. I have some other improvements in mind,
such as:

* Add some interface to properly *enable/disable* plugins
* Add the ability to install plugins from zipped files or remote sources
* Add a direct-connection (no proxy enabled) state

Finally, I'd like to say how much I enjoyed coding a small application for the GNOME
desktop. Gtk+ toolkit is perfect for this kind of task, with widgets which helps
enforcing GNOME HIG making an application fits properly in the desktop environment.
Also, Vala as a language it's really powerful and composite widget templates from Gtk+
really speed up the development process. The code of **GProxies** is pretty simple,
and I guess it would make a nice reading for GNOME developer beginners.

Below, I present some screenshots. I hope someone will find this useful.

{{ 'gproxies_march_2014' | image_list }}

[1]: https://www.gnome.org
[2]: http://www.gnome.org/projects/NetworkManager
[3]: https://github.com/erick2red/gproxies
[4]: http://www.gtk.org
[5]: https://github.com/erick2red/gproxies/blob/master/PLUGINS.md
[6]: https://github.com/erick2red/gproxies-plugins