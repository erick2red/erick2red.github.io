---
layout: post
title: "A week for releasing"
date: 2013-07-16 08:16
comments: true
categories: [Gnome, Contacts]
---

I've set a deadline for doing a weekly post. Well, last week, I missed. It was a really busy week.
I was building a web-page, and I haven't done that in like 10 months or something so I had a lot to
catch up. It was fun, though. I'm right now looking forward to start playing around with [Rails 4][1]

On the other side my Gnome duties caught up with me. I've been acting as Contacts maintainer with Alex for
a while now. But so far, he was doing the releasing part. I, usually, implemented new stuff, reviewed patches,
fixed bugs, but no more. This past week since Alex is on vacation I had to fill in doing the release.

I was prompted by Matthias Clasen for doing a release of gnome-contacts for the 3.9.4 development release of [Gnome][2].
In the process, I realized I can't log into master.gnome.org for the purposes of uploading the packages. So,
Mathias kindly offered himself to upload the package. Later I got my account authorized, and I was able to release
gnome-contacts-3.8.3. This latter one, I was surprised to see it packaged this morning into Archlinux.

These two releases are only bug fixes, yet there nasty bugs fixed there. You can read the [NEWS][3] file, or
the [commit log][4] to find about.

What's next on Contacts ? I want to implement New Contact flow inline and get rid of dialog, and close a
bunch of bugs around it. Also, yesterday I got a bug from Allan about updating a bit the selection pattern, and I
would like to work on it.

**Update**: I have to thanks Jonh Wendell for his fix of the webcam bug.

[1]: http://weblog.rubyonrails.org/2013/6/25/Rails-4-0-final/
[2]: http://permalink.gmane.org/gmane.comp.gnome.devel.announce/339
[3]: https://git.gnome.org/browse/gnome-contacts/tree/NEWS
[4]: https://git.gnome.org/browse/gnome-contacts/log/
