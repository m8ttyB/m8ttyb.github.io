---
title: Jython - How to install Python libs
date: 2010-04-02
draft: false
tags: ["IBM", "python", "coding"]
---
This wasn't immediately obvious to me even though in hindsight it makes
sense. Without putting much thought into my first attempt I numbly
typed *python setup.py install*. My goal, to use both the [twitter](http://code.google.com/p/python-twitter/)
and the [simplejson](https://pypi.org/project/simplejson/) (a dependency for the twitter api) apis from my
Jython scripts. I quickly discovered that to explicitly install these
libraries for use in Jython you need to run their *setup.py* scripts
explicitly from Jython.

To get started download the *src* code of the Python libraries that you
want to install.

From the command-line (I'm on a Mac OSX 10.5):

```bash
cd python-twitter-0.6/
jython setup.py installcd simplejson-2.1.1/
jython setup.py install
```

I have to give a shout out, Jython rocks my socks!

Now onto solving the next problem, how to get Eclipse (pydev) to resolve
the imports correctly?
