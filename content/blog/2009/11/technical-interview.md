---
title: "Technical Interview"
date: 2009-11-11
draft: false
tags: ["technical interview", "IBM", "testing"]
---

I've recently had the discomfort of being on the other end of the
interview table, the side asking the questions. A couple of warm up
questions that I've posed to would be candidates left me a bit surprised
and underwhelmed.

I should note that I personally hate interviews that the interviewer
bases success solely on your ability to write the solution that they're
looking for. That said we'd recently gone through a very frustrating
bout with a few contractors who claimed they loved to test but also have
the handy ability of writing code. There resumes clearly stating that
they were _very qualified_. The last bit we learned quite painfully was
an _over-exaggeration_ on their part.

The below may feel like a simple questions, but we found that they helped
us quickly assess if a candidate should make it to the next round of
interviews.

Using any language -  the job description notes we're
Python fans:

- Write a simple loop to iterate over a dictionary and print out key,
   value pairs?
- Write a simple loop to iterate over a list and print out the values?
- What's one method to print out the values from 0 - 10?

```python
    map = {1:"one",2:"two",3:"three"}
    for k,v in map.iteritems():
      print k,v
    
    list = [1, "two", 3, "four"]
    for item in list:
      print item
    
    for i in range(11):
      print i
```

The above code is really nothing special but its a reminder the importance of
understanding *base* concepts of how to access data structures.

## Below is a quote from my mentor

"You really need a strong resume to get an interview, and to get past
the phone interviews you need to have a solid understanding of data
structures (lists, queues, stacks, trees, maps, hash tables, sets),
their implementations, big-O order of operation, which one to use in
what situation, etc. I strongly recommend reviewing your data structures
text book. I also recommend reading the book `Object-Oriented Design and
Patterns` by Cay Horstman, as object oriented design, design patterns,
and their use are also important topics that get covered in the
interview. Another useful book is `Programming Interviews Exposed` as it
gives some insight into the kinds of questions you might get asked and
the way to handle them."

"I can't stress this too strongly - I reviewed data structures (including
some obscure ones) in these books and it really helped me in the
interviews. Based on my experience here and at Google, the questions
will be very detailed, you will need to be able to think on your feet,
and you need to be able to justify your design decisions (why did you
use that data structure? Do you really need that class? etc.)"
