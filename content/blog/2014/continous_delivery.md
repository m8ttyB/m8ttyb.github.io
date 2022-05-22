---
title: "Continuous Delivery Discussion"
date: 2014-10-27
draft: false
---

Many of Mozilla’s web projects have adopted concepts from [continuous delivery](https://en.wikipedia.org/wiki/Continuous_delivery).
Once a code change lands on a master branch it is shepherded to production with
little-to-no human intervention. Quickly getting new features out to users.

![Definition of a participant](/blog/2014/Continuous_Delivery_process_diagram.png)

While the implementation details vary from project to project, the core concepts
tend to remain the same between projects. It is important for project stewards;
project managers, developers, testers, UX, community, etc to have a shared
vocabulary.

The purpose of this document is a starting point for this conversation.

Why we do this
==============

We find that being able to move quicker allows us to create higher quality
software with limited resources. This fits our goal of creating features that
our users want, [working in the open](https://www.mozilla.org/mission/), and experimenting with our development
practices.

Continuous delivery has worked for us because of the culture of our community.
We accept shared responsibility for the quality of our work, are strong
collaborators, and have a culture that is supportive of learning from mistakes.

Over time we've become more confident in the quality of our work. Moving quickly
allows us to take a more proactive stance towards the quality of the products we
create as well as mitigating risk when creating new features.

What are QA’s responsibilities
------------------------------

* Acting as a user advocate
* Leading discussions that identify and assign risk to user stories
* Driving exploratory testing efforts

What are the dev team’s responsibilities
----------------------------------------

* Developing new features for the project and implementing fixes for issues
* Developing unit tests
* Maintaining code quality via code review and documentation of standards

Shared responsibilities
-----------------------

** project management, developers, qa, UX, l10n **

* Deep knowledge of the application and the ability to identify areas of risk
* Provide feedback on feature definition to the project manager and team
* Identifying, creating, and maintaining the end-to-end test automation
* Developing the appropriate mitigation strategies to lower risk of regressions landing in production
* Staying current on best practices and pushing the team to explore the applied relevancy
* Continually discussing and fine-tuning test coverage
* Opening avenues for community collaboration on the project

How we do this
==============

The basic concept is that any code that lands on the master branch should be in
a shippable state. New feature work should be categorized by the
amount of risk the feature or change to the code base poses.

Risk-based assessment
---------------------

The team discusses and agrees upon a risk metric that they are comfortable with
assuming. Assumed risk isn’t a static conversation, but it is good to build a
skeleton that the team can agree on and enhance over time.

A product should be broken into user stories so a team can apply a
hierarchy of risk. It is important to identify and engage as many stakeholders
as possible, each individual will have a unique and important point of view.

The list of stakeholders often include; project managers, developers, testers,
UX, IT, DBAs, the security team, etc. Sometimes the product has high enough
visibility in the organization that a proxy for the CEO is beneficial.

User stories and features are grouped into three categories
-----------------------------------------------------------

* Features that users should never see broken in production
* Features that can go to production broken but need to be discovered and fixed within a specific time period
* Features that can regress on production and will be fixed once a user identifies the problem

An example from mozillians.org
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* The ability to create a new account should never go to production broken
* The ability to edit the profile of an existing account can break but we want to discover and patch the problem within 1 hour of introducing the regression
* CSS layout can regress on production and will be fixed when a user uses the in-site feedback form to notify us

Feature testing
---------------

Not a dogmatic rule, in general new features or defect fixes should be manually tested and verified. Exploratory testing is the rule. Depending on the risk assigned to a feature, projects  have the option of using [waffle flags](https://en.wikipedia.org/wiki/Feature_toggle). The use of feature flags allows us to hide features from the general public while allowing a team or a group of beta testers to test on production with real data.

An important note about feature verification
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If a feature is deemed as low risk and it's functionality is easy to verify, anyone on the team is
empowered to test and verify it. If it is a feature that requires deeper investigation and poses
high risk, the QA team is the group who verifies it. If a feature set is big or user workflow
is changed it is important to engage the test team to flush out defects and usability concerns.

Automate tests
--------------

Feature work that falls into the category of “better never break” should see a heavy amount of exploratory testing before it is exposed to the public. Once the feature is baked and code churn has decreased, we seek an appropriate level of coverage using Selenium, Python’s Requests API, load testing, and fuzzers.

A component of the QA team automation strategy should also includes reviewing the amount of test coverage that follows a developers pull request as well as taking part in the code review. Often times the test team is able to influence and enhance the depth of coverage by simply asking for more developer level tests [Unit + Integration].

User feedback
----------------------

Sites should always have an easily discoverable method for users to submit feedback and file bugs. The team must be studious about responding to requests for help in a timely manner and do so with empathy.
