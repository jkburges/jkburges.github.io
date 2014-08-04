---
layout: post
title: On the road to Continuous Deployment at IMOS
comments: true
---

Although it can seem overwhelming, one of the encouraging things about adopting continuous deployment (apart from the benefits that it brings in itself) is that it can be achieved incrementally.  Below, I'll give a synopsis of the current state of play at [IMOS]() - the processes and methodoligies that we follow already and those areas where we still need to change.

Hopefully summarising what we've done already and how it has benefited us will help to convince others that it's worth ticking these final few things off.


# Why
There's plenty written on the benefits of CD (right?), but how do they specifically apply to our organisation?  Here are a few examples:

* currently, it often takes several weeks to realise the value of an idea into deployed, production software and often there's confusion over whether a particular feature is "done";
* a lack of time to spend on "innovating" (i.e. implementing those features which people *think* would be a good idea but there never seems to be time for); and
* it's common to have to manage backporting of critical bug fixes to already deployed versions (instead of just fixing on master and deploying)

# The Good...
## Continuous Integration
We practise [proper continuous integration]().  Every change has to be submitted as a [github pull request](), which is then reviewed, updated if necessary, and then merged in to master, as long as [travis]() is green (of course!).

We strive to keep PRs small, so that people are having their code merged in to master at least on a daily basis or so.  There is an understanding in the team that anything that goes in to master *could* become a release candidate so it has to work.

## DevOps Culture
In the last year or so, we've completed a migration to managing our infrastructure with [chef]().  We took this opportunity to get all of the developers on board with tools such as [vagrant](), so that we now have developers using [production-like environments]() to do much of their work.

The upshot of this is that it releases are low ceremony events, with it being trivial to promote applications from development right through to production, with relatively few suprises.  There is now really good collaboration between developers and operations to keep the infrastructure code working well for all.

## Version Control Everything
Apart from putting all of our source code under version control, we've started to version control some other things in the last couple of years.

We use [liquibase]() to manage database migrations and all of this ends up in version control.  We even manage our [geoserver content in git].  This last one has been really interesting.  There was initially a bit of resistance from content managers when we brought this in, as it seemed as they were being slowed down, having to check things in to git, submit PRs, wait for reviews etc.  What we actually find now is that many more people can contribute to geoserver content concurrently *and* it's now trivial to re-create test or production environments in a sand-boxed VM for development purposes. Version control FTW!
 


# The Bad...
* CI
	* jenkins
		* it works (most of the time), but messy config.  Needs more investment here
		
		
# ...and The Ugly

# Before

# Current/Good

	

	* travis
		* fast feedback for broken tests in PRs
* Content management via git (some) - e.g. geoserver https://github.com/aodn/geoserver-config	

# Future/Bad
* manual acceptance tests
* poor collaboration between developers and QA
* automated acceptance tests
	* a select set of well targetted happy-path use cases
	* maps - very visual - screen shots for a manual, but *fast*, test gate
* vagrant/development envs
* CI
	* generate jobs/pipelines using chef to achieve consistency, remove redudancy
	* GoCD?
* rollbacks/restores from backup
	