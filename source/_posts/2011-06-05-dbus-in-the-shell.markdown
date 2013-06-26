---
date: '2011-06-05 01:09:27'
layout: post
slug: dbus-in-the-shell
status: publish
title: DBus in the Shell
wordpress_id: '43'
categories:
- Coding Samples
tags:
- DBus
- Gjs
- Gnome Shell
- JavaScript
---

As a companion of the post [I made before](http://erick2red.wordpress.com/2011/05/28/dbus-in-gjs-and-gnome-shell/) on connecting to DBus exported objects on Gjs JavaScript runtime over the Shell, should be other post talking as how to export object over DBus inside Shell JavaScript environment. More or less this one is about that.

The code I will show here isn't mine, even I think I contribute some part to it, the main architecture of the thing was already made by the time the code get to me. That part of thanks for the help, and the guidance should go to Vamsi Krishna Brahmajosyula.

So, let’s get our hands dirty

First you need to create the interface you will export, similar to the way you made before for getting proxy objects.


[sourcecode language="javascript"]
const LoggerIface = {
    name: 'org.gnome.Logger',
    methods: [{
				name: 'log',
				inSignature: 's',
                outSignature: ''
              }
           ],
    signals: [{
				name: 'added',
                inSignature: 's'
			}],
	properties: []
};
[/sourcecode]

As you can see, we'll planning to create an object exporting one method 'Logger::log' and one signal 'Logger::added'. The method has one parameter and no return value, and the signal has one arguments, in both cases the argument/parameters is the same, the message log sent. So, our objects will have and array of log messages sent to him, and for every time you call its method 'log' it emit the signal added. Very simple.

Now we'll create the object conforming the interface we defined before.


[sourcecode language="javascript"]
function Logger() {
    this._init();
}

Logger.prototype = {
    _init: function() {
		this.logs = [];
        DBus.session.exportObject('/org/gnome/Logger', this);
    },

    log: function(message) {
		this.logs.push(message);
		global.log('Received: ' + message);
		this._emitAdded(message);
    },
    
    _emitAdded: function(message) {
        DBus.session.emit_signal('/org/gnome/Logger',
                                 'org.gnome.Logger',
                                 'Added', 's',
                                 [message]);
    }
};
[/sourcecode]

As you see we defined a method with the same name we did in the interface and the same signature. And now we'll pass to see the Gjs DBus magic:


[sourcecode language="javascript"]
DBus.session.exportObject('/org/gnome/Logger', this);
[/sourcecode]

This was called inside our _init method, and that’s the method of the Gjs telling DBus, this is the object we want to export. And, later on this one is the one telling DBus to check if our class/prototype conforms the interface we said it conforms:


[sourcecode language="javascript"]
DBus.conformExport(Logger.prototype, LoggerIface);
[/sourcecode]

Finally the extension entry point. Ohh wait I didn’t said that before, we’re reviewing an extension, as this is a so easy way to test Gnome 3 stuff. So the entry point, the main function.


[sourcecode language="javascript"]
function main() {
	// I did this to get the object after the extension code run
	Main.logger = new Logger()
	
	//this was the missing instruction
	DBus.session.acquire_name('org.gnome.Logger', 0, null, null);
}
[/sourcecode]

And here is a great trick, you see that line saying "this was the missing instruction", yeah I had all the code written and reviewed like a thousand times, and still doesn't worked, I keep testing but nothing the object wasn't there. I opened Looking Glass, and typed Main.logger and the object was create, but it wasn't receiving any notice when I called the method, actually I was getting errors cause that method doesn't existed at all. After some back and forward between DBus documentation, and Gnome Shell source code, I figured out. The missing part was, of course, I’ve should known, the part when your object grab the DBus name it want to publish. So first I went to **DBus.session.own_name** but that is not a function, and then I tried this one **DBus.session.acquire_name**, with doesn't exist at all in the devhelp documentation but it seems is the wrapper of Gjs for the **own_name** GDBus method.

So, that’s all for now. I know I’m still missing some things. I want to learn how catch a signal emit over DBus, and how to handle exported object properties, make some samples and toy with it a bit.

But that will be later. Keep in touch.




BTW: I'm still figuring out how to publish the full examples, but I will do it, sometime
