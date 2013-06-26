---
date: '2011-05-31 12:51:56'
layout: post
slug: json-parse-and-json-stringify
status: publish
title: JSON.parse and JSON.stringify
wordpress_id: '33'
categories:
- News
tags:
- Extensions
- GLib/Gio
- Gnome Shell
- JavaScript
- Open Source
---

The Gnome Shell Extensions system is one mighty and powerful beast. Just yesterday I found myself wanting to have a simple list of stuff I wanted to check later. After looking around a bit to storage components in Gnome Shell environment I decide I'll use simple files with [JSON](http://en.wikipedia.org/wiki/Json) strings inside.

I started coding and in a while I got my [wish-list](http://erick2red.wordpress.com/my-projects/shell-extensions/#wish-list) extension. The  beauty of this is what I learnt in the process. Two stuff I never tried, before:



	
  * Hacking with autotools: This is the kind of things you should know once you decide to get involved in software development for Linux.

	
  * How to hack a new kind of PopMenuItem for the Shell UI


Since I have access to the facilities of JavaScript and GLib system from the extension I can do almost everything with it.
I used a file as a back-end for my list of wishes and  use JSON.parse for reading and parsing the content back into an array and JSON.stringify to dumps the objects into the file. You can go down through the code later. Anyway there's those two snippets

Using GLib/Gio and JSON
[sourcecode language="javascript"]
let dir = Gio.file_new_for_path(Main.ExtensionSystem.extensionMeta[UUID].path);
let listFile = dir.get_child('lists.json');

let raw = listFile.replace(null, false,
					Gio.FileCreateFlags.NONE,
					null);
let out = Gio.BufferedOutputStream.new_sized (raw, 4096);
Shell.write_string_to_stream (out, JSON.stringify(this.list));
out.close(null)
[/sourcecode]

Instantiating PopupMenuItem
[sourcecode language="javascript"]
PopupEntryMenuItem.prototype = {
    __proto__: PopupMenu.PopupBaseMenuItem.prototype,

    _init: function (style) {
        PopupMenu.PopupBaseMenuItem.prototype._init.call(this, { activate: false });

        this.entry = new St.Entry({ style_class: style });

		this.entry.clutter_text.connect('key-press-event', Lang.bind(this, this._onKeyPress));
        this.addActor(this.entry, { span: -1, expand: true });

		this.actor.add_style_class_name('menu-entry');
    }
}
Signals.addSignalMethods(PopupEntryMenuItem.prototype)
[/sourcecode]
The page is linked above. See you around.
