---
layout: post
title: "Getting warnings out of Clutter"
date: 2012-04-28 23:59
comments: true
categories: [Gnome, Clutter, Code]
---

It's been a while since I blogged anything. Well, I actually did some ranting
about starting this new blog. But that really doesn't count. I should make a
post saying what I've been up to. But I'm lazy. I've been:

- Learning some Qt (and I doesn't like it, maybe I tell something about this
later)
- Using [SleekXMPP](https://github.com/fritzy/SleekXMPP) python library
- Learning ruby (Did I said this already ?)
- Learning [Clutter](http://www.clutter-project.org), and here we stop.

While I was trying to pull some code using Clutter and Gtk+, before you ask, I
was trying to put GtkWidget inside Clutter Actors to get some fancy animation, I
got into some minor troubles, which I think those newbies like me out there
should know.

First, if you read the blog of the might Emmanuele Bassi (Clutter's author and
main developer) you then came across this post 
[Had a dream](http://blogs.gnome.org/ebassi/2012/03/17/had-a-dream/). I did, and I wanted to get
those warning they talk about so my code would need to be modified pretty much
anything when those API get deprecated. Well, I didn't, I mean I used 
deprecated API and my compiler wasn't throwing me warnings. Someone told me 
that I needed to use a decent set of compilers flags, and I tried, nah, still 
nothing. I went into IRC, I found help there and after a while, I was told to 
add this to my compiler options
{% codeblock %}
-DCLUTTER_VERSION_MIN_REQUIRED=CLUTTER_VERSION_1_10
{% endcodeblock %}
This finally solved the issue and I'm having pretty warnings  every time I use
an old method which should be destroyed in a couple of months.
