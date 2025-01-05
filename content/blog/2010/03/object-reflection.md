---
title: "Object Reflection"
date: 2010-03-22
draft: false
tags: ["IBM", "testing", "python"]
---
Last week I was working on understanding a problem and spent a few minutes
exploring reflection. My intention was to populate an array of objects
and using reflection dynamically find and call the methods that
interested me. After digging around Python's internals for a bit I came
up with this.

```python
class InspectorGadget(object):  
  def __init__(self, quarrel_with):
    self.__evil_antagonist = quarrel_with  
    
    def go_go_gadget_hat(self):
      ... deploy hat  
    
    def go_go_gadget_arms(self):
      ... do that crazy arm thing  
    
    def meet_penny_and_brain_for_icecream(self):
      ... magic happens here

if __name__ == '__main__': 
  # a list for inspector objects ready to save the world
  inspector_list = list()
  inspector_list.append(InspectorGadget("Dr. Claw"))
  inspector_list.append(InspectorGadget("The Cuckoo Clockmaker"))

  for inspector in inspector_list:
    # get a list of the object's public methods
    methods = inspector.__class__.__dic__.keys()  
    
  # iterate over the method list & call those that start with 'go_go_gadget'
  for method in methods:
    if method.startswith('go_go_gadget'):
      getattr(obj, method)()
```

Ultimately I discarded this solution in favor of one that fits my problem 
better but it was definitely a fun digression.

References: [wikipedia on reflection](http://en.wikipedia.org/wiki/Reflection_(computer_programming))