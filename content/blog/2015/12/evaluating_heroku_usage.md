---
title: "Evaluating Heroku Usage"
date: 2015-12-01
draft: false
tags: [oneanddone, heroku, aws, deployment, Mozilla, Open Source]
---
[oneanddone.mozilla.org](https://oneanddone.mozilla.org) is an application that is a vital ingredient to [Mozilla QA’s](https://quality.mozilla.org/) community strategy. At it's start, the project was originally initiated and designed by the community to address the problem of discovering meaningful ways to contribute to Mozilla QA. In its simplest form, oneanddone.mozilla.org allows community members to [browse project tasks](https://oneanddone.mozilla.org/tasks/available/) and upon finding one that resonates with them, begin working on it.

<img align="left" src="/blog/2015/12/oneanddone.png" width="50%" style="margin-right: 10px">

The development of the project as well as continued shaping of its features are handled both by the community as well as by the QA team itself. Project ideation and community engagement is handled by [Rebecca Billings](https://mozillians.org/u/rbillings/), [Karl Thiessen](https://mozillians.org/u/kthiessen/), and [Bob Silverberg](https://mozillians.org/u/bsilverberg/). Code management is accomplished on get [github.com/mozilla/oneanddone](https://github.com/mozilla/oneanddone). The deployment pipeline (stage, production, etc) and hosting are handled by heroku.com. Feature triage and ideation, in [bugzilla](https://bugzilla.mozilla.org/buglist.cgi?list_id=12899546&query_format=advanced&bug_status=NEW&component=One%20and%20Done).

For additional information see the [One and Done team wiki](https://wiki.mozilla.org/QA/OneandDone).

## Heroku vs. AWS
For a project of One and Done’s complexity and development requirements, Heroku was a natural fit. The creation and management of deployment workflows as well as [one-click installation of add-ons](https://elements.heroku.com/addons) are cleanly implemented.

![Heroku Pipeline](/blog/2015/12/workflow.png)

Going into 2016 we have seen many projects at Mozilla standardizing around AWS as it's web services delivery mechanism. Given this and that QA owns oneanddone.mozilla.org we felt it would be advantageous to consider a move to this platform.

### The problem space we wanted to explore

- Continue to use our application as a platform to understand CD and the deployment environments used by our developers
- Build on our knowledge base so we can offer timely suggestions of best practices to projects that are in the process of moving to AWS or may have already made the switch.

### Approaching the problem(s)

- Investigate the current workflow cases oneanddone.mozilla.org employs and map them to potential AWS equivalents.
- Create a working document that summarizes these findings, includes a pros and cons list.
- Identify teams that have domain knowledge and form relationships that should we want to move off of Heroku in the future, we can leverage their experience.

## What we discovered

Heroku’s platform is quite slick, particularly for projects — like One and Done — that need flexibility and are self-supported by small teams. Heroku also scales quite readily if needed, that said our usage of it follows simple patterns and a low amount of traffic.

Amazon's large array of services often times matched 1:1 the offerings presented by Heroku. However there is a bit of additional mastery that is required to use, configure, and deploy using AWS.

![AWS Pipeline](/blog/2015/12/aws_codepipeline.png)

### Heroku

- Creation of deployment workflow via a GUI is straightforward and quick.
- For pull requests that contain complex or experimental ideas, [Review Apps](https://devcenter.heroku.com/articles/github-integration-review-apps) are easy to deploy. One and Done employs the use of review apps instead of a dev instance.
- The ability to employ [travis-ci](https://travis-ci.org/mozilla/oneanddone) for [pre-deploy hooks](https://docs.travis-ci.com/user/deployment/heroku/) if wanted.
- In instances where a [rollback](https://devcenter.heroku.com/articles/releases) is desired and the database hasn’t been affected, it’s a one-click operation.
- Ease of setting up [automatic backups](https://devcenter.heroku.com/articles/heroku-postgres-backups) of a database.
- Dataclips - the ability to create short or long-lived queries that return parseable results saves developer time. In some instances this can stand in for a read-only API.
- One-click install of add-ons - oneanddone.mozilla.org currently uses [Heroku Postgres](https://devcenter.heroku.com/articles/heroku-postgresql), [Heroku Scheduler](https://devcenter.heroku.com/articles/scheduler), [Memcached Cloud](https://devcenter.heroku.com/articles/memcachedcloud), [Postmark](https://devcenter.heroku.com/articles/postmark), [Papertrail](https://devcenter.heroku.com/articles/postmark), and SSL.
- Ease of ssh’ing into an instance to diagnose an issue
- Preconfigured dashboards, lot's of them.

### Amazon's Services

- Amazon has a competing product called CodePipeline
- There is no direct synonym for Heroku's Review Apps.
- The ability to employ travis-ci for pre-deploy hooks if desired.
- In instances where a rollback is desired and the database hasn’t been affected, it’s a one-click operation.
- Ease of setting up automatic backups of a database.
- Choice of setting up build pipelines and use of third party frameworks is near infinite.

### Pros and cons

The reasons for considering a move to an AWS solution is fairly straightforward.

#### Heroku

We're comfortable with Heroku. Using its platform offers a low barrier of entry for our team.
The ecosystem of extensions for Heroku meets our needs.

- [Application manifests](https://devcenter.heroku.com/articles/app-json-schema) allow teams to create a template of applications.
- A [platform API](https://devcenter.heroku.com/categories/platform-api) exists.
- On the negative, very few of the projects we collaborate on use Heroku and it's workflows.
- Both a pro and a con - there is one right way to configure a build pipeline.
- Self supported by the Web QA team.

#### AWS

- Transitioning to a solution that is powered by AWS would initially be a fairly high barrier to overcome.
- The ecosystem of extensions and external services match what is offered in Heroku.
- Leveraging AWS in our deployment and hosting workflows would bring us in line with what the majority of our other projects are using.
- We can follow patterns that other teams have used when combining Jenkins and other bits into their pipelines.
- AWS has [CodePipeline](https://aws.amazon.com/codepipeline/) - their version of a continuous delivery service. This isn’t used by our teams at Mozilla, thus I don’t think we should invest in it at this time.
- [Puppet](https://puppetlabs.com/solutions/aws) works well with AWS - our Operations team uses Puppet.
- Configuration to use third-party CI pipelines is near infinite.
- Mozilla is standardizing around AWS centric solutions. On this merit alone, as a team we should discuss if there is value for us to do so as well.
- Support from the Ops team as well as reproducible deployment patterns already exist at Mozilla.

## Next

<img align="left" src="/blog/2015/12/moving_forward.jpg" width="50%" style="margin-right: 10px">

The next steps need to be carefully considered, in the long term I believe it would be beneficial to move to AWS. This would help us gain direct knowledge of more complex deployment scenarios and frameworks, mechanisms which a majority of the projects we contribute on are already using. There would also be the option of garnering assistance from the Ops team in the form of support, support which we currently do not have while using Heroku.

The greatest challenge that needs to be weighed is time. Currently we have a solution that works for One and Done. To move to a new PaaS would cost us time. Several intermediary steps became clear and would allow us to make the transition smoother. In particular, we can leverage the work that other teams ar completed and reuse portions of it to build a workflow that meets our needs.

- Moving to AWS seems to add a lot of complexity for little immediate return. We should wait to see what practices other teams standardize on.
- Consider moving One and Done to Docker. This will make setting up and maintaining a development environments simpler. This also has the potential to help with portability of between PaaS environments. Ops has familiarity with Docker.
- Work with Ops to build a deployment workflow with Puppet, Jenkins, and Travis-CI.
Utilize artifacts created by other teams. The pipeline tiger team, a cross-team group, is working to create reusable templates that teams can use with AWS.
