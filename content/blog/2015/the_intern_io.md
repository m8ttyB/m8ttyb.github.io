---
title: "Make the intern do it"
date: 2015-01-15
draft: false
---

## A shared experiment

As a QA team we had a difficult and challenging quarter to end 2014 on; that said,
the work with the [MDN](https://developer.mozilla.org/) team was a personal highlight for me. A fulfilling way to cap
off the end of the year.

The goal was to implement an experiment: convert several Python-based tests into JavaScript. Place these tests into the build process that the developers use and see what it nets.

[David Walsh](http://davidwalsh.name/) and I evaluated [theIntern.io(https://theintern.io/) as an alternative to our Python based
[suite of tests](https://github.com/mozilla/mdn-tests/) that use WebDriver, Pytest, and [pytest-mozwebqa](https://github.com/mozilla/pytest-mozwebqa). The developers very much want to be involved in end-to-end testing using WebDriver as a browser automation tool but found the context switching between JavaScript and Python a distraction. [Web QA](https://quality.mozilla.org/teams/web-qa/)
wanted our test cases to be more visible to the developers and block deployment if
regressions were introduced into the system.

## The problem(s) we wanted to address:

* Scalability of test coverage - joint ownership of creation and maintenance of test cases.
* Moving test runs further upstream in the build process - providing
  quicker feedback.
* Test runs and reports are hooked into a build process that the dev team
  uses.

### Present

Presently, Web QA’s MDN tests are Python based -- [suite of tests](https://github.com/mozilla/mdn-tests/) -- and run on a [Jenkins](http://jenkins-ci.org/) instance which is firewalled from the public. Both of these components make
accessing and monitoring our test runs complex and an event that is separate from the developer workflow.

Developer-level unit tests run on a TravisCI instance and presently there is no hook that runs the Python based end-to-end tests if their build passes. The end-to-end tests run on a cron job and can also be started by an [irc bot](https://github.com/mozilla/mozwebqa-bot).

Additionally, our Python-based tests live outside of the developers project: [github.com/mozilla/mdn-tests](https://github.com/mozilla/mdn-tests/ and [github.com/mozilla/kuma](https://github.com/mozilla/kuma/). Adding an additional hurdle to maintainability and visibility.

### The New

Working closely with the MDN team I helped identify areas that an end-to-end testing framework needed to take into account and solve. Between David’s and my experiences with testing we were able to narrow our search to a concise set of requirements.

Needs included:

* Uses real browsers -- both on a test developer’s computer as well as the ability to use external services such as Sauce Labs.
* Generate actionable reports that quickly identify why and where a regression occurred
* An automatic mechanism that re-runs a test that fails due to a hiccup; such as network problems, etc.
* Choose an open source project that has a vibrant and active community
* Choose a framework and harness that is fun to use and provides quick results
* Hook the end-to-end tests into the build process
* Ability to run a single test case when developing a new testcase
* Ability to multithread and run n tests concurrently

David Walsh suggested [theIntern.io](https://theintern.io/) and after evaluating it we quickly realized
that it hit all of our prerequites. I was quickly able to configure the
MDN travis-ci instance to use [SauceLabs](https://docs.saucelabs.com/ci-integrations/travis-ci/). Using the included [DigDug](https://theintern.github.io/digdug/index.html) library
for tunneling, tests were quickly able to run on Sauce Lab’s infrastructure.


### Outstanding issues

* The important `isDisplayed()` method of Intern's [Leadfoot API](https://theintern.github.io/leadfoot/index.html), paired with the polling function, returns inconsistent results. The cause may be as simple as we’re misusing `isDisplayed()` or polling. This will take more investigation.
*  Due to security restrictions Travis-CI cannot run end-to-end Webdriver based tests on a [per pull request basis](https://docs.travis-ci.com/user/pull-requests/#Security-Restrictions-when-testing-Pull-Requests). Test jobs are limited to merges to master only. We need to update the [.travis.yaml](https://github.com/mdn/kuma/pull/2947/files) file to exclude the end-to-end tests when a pull request is made.
* Refactor the current tests to use the Page Object Model. This was a fairly large deliverable to piece together and get working, thus David and I decided to not worry about best coding practices when trying to get a minimally viable example working.

We’re well-positioned to see if Javascript-based end-to-end tests that block a build will provide positive benefits for a project.

## Next Steps

Before the the team is ready to trust using Leadfoot and Digdug we need to
solve the outstanding issues. If you are intestested in gettin involved,
please let us know.

* Remove the sleep statements from the code base -- this likely will involve better understanding how to use Promises.
* Get an understanding why some tests fail when run against SauceLabs -- the failures may be legitimate, we may be misusing the api, or their is a defect somewhere within the test harness.
* Refactor tests to use the Page Object Model.

After these few problems are solved, the team can include end-to-end into their
build process. This will allow timely and relevant per feature merge feedback
to developers on the project.