---
title: "A Little Webdriver Toss in Some Craigslist"
date: 2010-08-03
draft: false
---

![Craigslist Job Search](/blog/2010/08/screenshot_java.png)

A while ago I caught a wild hair to create a program to scrape CraigsList for tech jobs. [Chris McMahon](https://chrismcmahonsblog.blogspot.com/), fellow friend and creative QA extraordinaire is credited with turning me onto this idea. He created essentially the same script but in ruby, what a crazy simple notion. I tend to spend most of time in Python so I thought I'd jump ship back to my Java roots and try my hand at my own implementation.

I'll be honest... I was shocked at how reliant I've become on Python's built-in libraries and how much Java I've forgotten. This was a super fun side project and I bet I'll toss it into use. As I add updates to the code (this was my first draft) I'll make the code available for download ...

---

So I revisited the code that I originally wrote and did quite a bit of refactoring along with adding a stats counter that keeps track of pages visited and results found. The greatest inspiration came when my wife took interest and wanted to use it to do some job searches of her own, her only caveat was that she wanted the results to look a little prettier.

My next mini goal is to add the ability to parse arguments from the command-line so that it becomes a more general purpose tool. I think that I'll write a Jython wrapper to do this, I really like the simplicity of Python's [optparse library](https://docs.python.org/3/library/optparse.html).

I have this running on a daily cron. Each morning over breakfast I can surf a customized set of job results. Yay!

Git repository: [Craigslist Job Search](https://github.com/m8ttyB/JobSearch/blob/master/com/freecog/craigslist/CraigsList.java)
