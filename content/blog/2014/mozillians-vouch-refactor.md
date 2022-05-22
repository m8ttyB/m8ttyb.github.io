---
title: "Mozillians Vouch Refactor"
date: 2014-07-24
draft: false
---

# Mozillians Vouch Refactor - Testday

Welcome to the Mozillians project. We love working with old and new
contributors!

![Boulder, Colorado](/blog/2014/mozillians-home-page.png)

The latest feature set of the [community phonebook](https://wiki.mozilla.org/Mozillians) has landed
and is ready for testing. The phonebook is a place to find, share, and
communicate with open web advocates throughout the world.

The community has worked to find creative ways to make the phonebook a
place where [active and trusted memebers](https://hoosteeno.com/2013/12/17/a-new-mozillians-org-signup-process/) of the community can connect. The
latest set of features seeks to push new contributors and returning contributors to be
more active in shaping our community.

The new trust model:

- Anyone can create a Mozillians account.
- 1 vouch allows users to search for and view Mozillian profiles
- 3 vouches are needed to vouch for new Mozillians

A test environment is live on our dev server - mozillians-dev.allizom.org
- and ready for your critical eye. We welcome you to explore the features and offer
your [open, honest, and unabashed feedback](https://bugzilla.mozilla.org/enter_bug.cgi?product=Community%20Tools&component=Phonebook&status_whiteboard=[vouching%20rel.%201).


## Test Plan

There are two areas that you can get involved with helping test; feature
verification using [exploratory testing techniques](https://en.wikipedia.org/wiki/Exploratory_testing) and test automation coverage.

### Setup for manual testing

Here are the `unverified features` that needs to still need to be tested. Find
a bug that needs verification and begin testing.If you have questions reach
out in IRC - [#mozwebqa](https://widget00.mibbit.com/?settings=1b10107157e79b08f2bf99a11f521973&server=irc.mozilla.org&channel=%23mozwebqa) or [#commtools](https://widget.mibbit.com/?settings=1b10107157e79b08f2bf99a11f521973&server=irc.mozilla.org&channel=%23commtools) - and introduce yourself.
Myself or another community member will help you.

To get started you’ll need:

- Disposable email addresses so you can create test accounts on dev. I recommend free services like [Mailinator](https://www.mailinator.com/) or [10minutemail](https://10minutemail.com/).
- `personatestuser.org` is one of my preferred tools if you don't need to verify receipt of email but it forces you to parse a json file.
- Create an account on our [dev server](https://mozillians-dev.allizom.org).
- Some of the test scenarios will require a vouched account. You can automatically vouch yourself by appending ``/vouch`` to the url.
- For example ``mozillians.allizom.org/u/your_account_name/vouch``.
- Alternatively you can unvouch yourself by appending ``/unvouch`` to the url.

#### Filing Bugs

- Write good bugs that provide clear steps to reproduce the problem. Read [this document](https://bugzilla.mozilla.org/page.cgi?id=bug-writing.html) for tips.
- Use [this form](https://bugzilla.mozilla.org/enter_bug.cgi?product=Community%20Tools&component=Phonebook&status_whiteboard=[vouching%20rel.%201) to file new bugs.
- [Bugzilla etiquette](https://bugzilla.mozilla.org/page.cgi?id=etiquette.html) - be polite and treat people with respect, we are a friendly community.
- [IRC etiquette](https://quality.mozilla.org/docs/misc/getting-started-with-irc/) - same as Bugzilla; relax and have fun.

### Setup for test automation

Creating test automation assumes that you have a bit of experience writing code and working with version control.


![The new Mozillians.org](/blog/2014/mozillians-tests-readme.png)
* [Mozillians Tests](https://github.com/mozilla/mozillians-tests)

Getting started:

- Work through the [mozillians-tests project ReadMe](https://github.com/mozilla/mozillians-tests/blob/master/README.md)
- Review the [open git issues](https://github.com/mozilla/mozillians-tests/issues?labels=Community&page=1&state=open) for the project and assign one to yourself
- Talk to us in the [#mozwebqa](https://widget00.mibbit.com/?settings=1b10107157e79b08f2bf99a11f521973&server=irc.mozilla.org&channel=%23mozwebqa) irc channel. We have a popping fun community that is energetic and would be thrilled to work with you.

We really appreciate your enthusiasm and help with making the community
phonebook better. This is fully a community initiative and wouldn't exist
without you.

I look forward to seeing you online!

Matt Brandt

https://mozillians.org/u/mbrandt
