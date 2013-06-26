---
layout: post
title: "A challenge of locales"
date: 2012-10-09 09:41
comments: true
categories: [Linux, Code]
---

When developing an application aiming for everyone worldwide to use, there are certain challenges
you will face. Right now, mine is fighting laziness and locales.

I'm developing some widget for specifying a date and I've been struggling with date formats across locales
So, after reading a huge number of manpages, source-code, and so on I stumble on this code.

    #include <langinfo.h>
    #include <locale.h>
    #include <stdio.h>
    #include <stdlib.h>

    int
    main(int argc, char *argv[])
    {
	  setlocale(LC_ALL,"");
      printf("%s\n",nl_langinfo(D_FMT));
      exit(EXIT_SUCCESS);
    }

This will show me your date format according to your locale.
What I will ask, compile and run this code in your pc, and post the answer here along with your locales.

See you around.

*Update*: Thxs for the comments. Two things: I've just updated the code, thxs to Tom, adding **setlocale** line made the code works. Second: Thxs to Tommi (first comment). That was the info I was looking for.
