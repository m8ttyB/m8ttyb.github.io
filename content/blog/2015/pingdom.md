---
title: "Pingdom"
date: 2015-12-31T09:36:44-06:00
draft: false
---

Monitoring
===========

The fourth quarter of 2015 for our team at Mozilla was a really great one. Across the group we had a well
rounded list of goals supporting project initiatives as well as ones that were forward facing.
Among the forward facing goals, a few of the areas included improving our code quality and the
exploration of new techniques and technologies for assessing project health.

I personally gravitated towards the latter when I was considering a deliverable. Over the past
several years I've been keen on understanding the various roles that monitoring and logging
tools play when accessing the health of complex software systems. I also want to see a checklist of
practices emerge that our team can use when assessing a new or existing project.

Enter Pingdom
==============

![Pingdom](/blog/2015/pingdom_logo.png)

For those that aren’t familiar with [Pingdom](http://pingdom.com), They provide several services. The one that I was
interested in was [uptime monitoring](https://www.pingdom.com/product/uptime-monitoring); a simplified description might be a service that on a
chosen time interval pings a specific URL to see if the server responds with an acknowledgement
that the resource can be served. If the server responds, Pingdom happily logs that the service can
be reached. While this may not sound very interesting, no news is something of a good thing when
uptime is involved. The interesting part of this story begins when a service is unreachable and Pingdom
notices this.

Pingdom will immediately initiate what they call a root cause analysis, or simply, servers they run
that are hosted in different locations will try to ping the offending URL. The clever goal in this
is that Pingdom is trying to determine if the outage your service is experiencing is specific to a
region or is global. Once the scope of the outage has been probed, Pingdom can be configured to
send a notification to specific users, via email or text message.

When configured to hit the appropriate endpoints, an uptime tool such as Pingdom is a good catchall
to check if a service is having uptime difficulty. It’s a bit of a brutish tool insofar as it’s not
very good at helping debug a problem and should be used as a notifier that “something” went wrong and
requires deeper investigation.

At this point I stopped my investigation; I had a working setup using a private instance of Pingdom that
touched both our production and stage environment for [oneanddone.mozilla.org](https://wiki.mozilla.org/QA/OneandDone). I also had a steady stream
of data and had a feel for how and where Pingdom is useful. OneandDone is hosted on [Heroku](http://heroku.com), a colleague
learned that their is a push button solution for Pingdom. At this point we decided to forego using [Datadog](https://www.datadoghq.com/)
in leu of the Heroku integration. We also decided to use Heroku’s integrated Pingdom mechanism instead due
to built in simplicity as well as billing cost centers.

Final thoughts
===============

An important discussion that QA teams need to consider is formulating opinions and expertise around monitoring
solutions. These need to become an additional dimension that is included in test plans.

Understanding available strategies and how to advocate for their inclusion on a project is vital. In my opinion,
it’s also important for test practitioners to know how to configure and implement these solutions. In the
continued move towards the dev/ops world of thinking, QA teams have additional areas of expertise that they need
to be investing in.

If you’re interested, here is the specific goal I submitted:

> As real-time monitoring solutions become the norm it's important that QA stay abreast of the
> latest data driven monitoring techniques. The use of Pingdom coupled with dashboards can be used
> to reduce risk on projects.
> 
> This goal seeks to pair these tools and implement them on our own, team supported web project.
> Once this is done, we can use our experience to make suggestions for other projects as well as have a
> better understanding how to integrate this data feed with our existing testing plans.
>
> * Add monitoring to oneanddone.mozilla.org [production and stage] - Integrate Pingdom and DataDog
> * Add an uptime check to Pingdom that includes downtime and root cause analysis
> * Integrate Pingdom reporting with DataDog
> * Write a blog post that discusses outcomes and recommendations for other projects. The post will broach the topic of dev/ops and qa/ops roles.
