---
date: '2011-05-28 21:52:58'
layout: post
slug: dbus-in-gjs-and-gnome-shell
status: publish
title: DBus in Gjs and Gnome Shell
wordpress_id: '11'
categories:
- Coding Samples
tags:
- DBus
- Gjs
- Gnome Shell
- JavaScript
---

###### The Story




Last few days I’ve got a bunch of ideas about Gnome Shell extensions. Gnome 3 platform has made the development of Gnome really close to the public, more than anything by using JavaScript to made the Shell.




The use of JavaScript has lower the barrier for making contributions for Gnome Desktop, because JavaScript it's a well known language in the web development world an it’s a easier language to learn than the typical C/C++ and even Python. There’s still some things to address about the use of JavaScript, the definition and standardization of an engine for use in the gnome development (look at this discussion about the use of [Seed](http://live.gnome.org/Seed) or [Gjs](http://live.gnome.org/Gjs) [1]), the stabilization of some interfaces in the actual engines, but there’s a lot of way made already.




About standardization and stabilization of interfaces goes this post. [DBus](http://dbus.freedesktop.org) is a clever way of communication between process that has become almost a must have for every desktop application for providing services and hooks to internals API, the JavaScript engine powering Gnome Shell then, needs a DBus interface and here comes the problem.




When I realize of my need of using DBus when writing some Gnome-Shell extension I go through the process of selecting which DBus interface I will use. First I decide to use GLib [GDBus](http://developer.gnome.org/gio/2.26/gdbus.html) interface since it goes inside GLib, so no matters which JavaScript engine you use, as long as it has GLib bindings it should work. I avoid the low-level interfaces, and I got the Devhelp documentation to work with so, It just left start coding.




After a while I realize that not everything worked as I thought, first and the biggest problem I had was the need of using GLib.Variant in JavaScript to define the signature of the methods you call over DBus and the fact the Gjs not implement GLib.Variant support or at least as long as I can find. I tried with almost every method declared in the GLib-2.0.gir file which is suppose to provide the introspection data, no one works. After asking in the JavaScript mailing list, and got no answer, I decide I should give up and use DBus interface provided by Gjs.




###### The Code




After looking around, I decide to go the only way, reading Gnome Shell code.




For calling a method exported over DBus using Gjs the first is import (include, load the code) the unit containing the DBus interface



    
    <span class="kwrd">const</span> DBus = imports.dbus;




This code loads the JavaScript file dbus.js. After that you need to declare the interface you’re trying to reach and the signature of the methods you’re planning to call



    
    <span class="kwrd">const</span> NotificationRemoteInterface = {
        name: <span class="str">'org.freedesktop.Notifications'</span>, <span class="rem">//interface name</span>
        methods: [
            {    <span class="rem">//method name and signature</span>
                name: <span class="str">'Notify'</span>,
                inSignature: <span class="str">'susssasa{sv}i'</span>,
                outSignature: <span class="str">'u'</span>
            }
        ]
    };




Now it comes the Gjs-DBus implementation magic, you need to call this method from DBus object tha makes a class of the proxy object you’ll need to instantiate



    
    let NotificationProxy = DBus.makeProxyClass(NotificationRemoteInterface);




After that you’ll be ready to get the proxy object and and call its method, or at least the ones you declared.



    
        let notification_proxy = <span class="kwrd">new</span> NotificationProxy(DBus.session,
                                                        <span class="str">'org.freedesktop.Notifications'</span>,
                                                        <span class="str">'/org/freedesktop/Notifications'</span>);
        notification_proxy.NotifyRemote(<span class="str">'my_app_sample'</span>,    <span class="rem">//these are the params of the method called</span>
                                        0,
                                        <span class="str">'dialog-info'</span>,
                                        <span class="str">'Summary of the notification'</span>,
                                        <span class="str">'Big Body of the sample Notification'</span>,
                                        [],
                                        {},
                                        120,
                                        <span class="kwrd">function</span>(result, err){
                                            log(<span class="str">'This is the result: '</span> + result);
                                            log(<span class="str">'This is the error: '</span> + err);
        });
    




Well, and that’s it. After that you called the method and you have the callback set there, so you can what you want with the returned values.




###### The conclusion




I know there still a bunch of stuff you can make with DBus, but this is a start. There’re still watching for a bus name, and watching for signals and properties changes. It will come soon, I hope.




* * *


[1] [Gjs vs Seed](http://mail.gnome.org/archives/desktop-devel-list/2011-April/msg00147.html)



