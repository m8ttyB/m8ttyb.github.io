---
title: "Python Update Your Eggs"
date: 2010-04-12
draft: false
tags: ["python", "college", "IBM"]
---
Quite awhile ago I needed an easy way to update all my Python eggs, this
is tedious at best especially if you didn't explicitly keep track of
what you've installed over time. The script below isn't mine (I've
made a few subtle changes) but it is invaluable. I've been using it for
the last 8-months without fail.

Many thanks to [Flávio Codeço Coelho](http://pyinsci.blogspot.com/2007/07/updating-all-your-eggs.html) for making his script available online.

```python
#!/usr/bin/env python
from setuptools.command.easy_install
import main as install
from pkg_resources import Environment, working_set
import sys

#Packages managed by setuptools
plist = [dist.key for dist in working_set]
def autoUp():
  for p in Environment():
    try:
        install(['-U', '-v']+[p])
    except:
        print "Update of %s failed!" %p
    print "Done!"
        
def stepUp():
  for p in Environment():
    a = raw_input("updating %s, confirm? (y/n)" %p)
    if a.upper() =='Y':
      try:
          install(['-U']+[p])
      except:
          print "Update of %s failed!"%p
    else:
        print "Skipping %s"%p
    print "Done!"
    print "You have %s packages currently managed through Easy_install"%len(plist)
    print plist

ans = raw_input('Do you want to update them... (N)ot at all, (O)ne-by-one, (A)utomatically (without prompting)')

if ans.upper() == 'N':
  sys.exit()
elif ans.upper() == 'O':
  stepUp()
elif ans.upper() == 'A':
  autoUp()
else:
  print "Oops, you chose a non-existant option. Please run the script again."
```
