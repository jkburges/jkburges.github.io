---
layout: post
title: On the road to Continuous Deployment at IMOS
comments: true
---

Although it can seem overwhelming, one of the encouraging things about adopting continuous deployment (apart from the benefits that it brings in itself) is that it can be achieved incrementally.  Below, I'll give a synopsis of the current state of play at [IMOS](http://www.imos.org.au/) - the good, bad and the ugly of where we're at on the road to adopting CD.

Hopefully summarising what we've done already and how it has benefited us will help to convince others in my team that it's worth ticking these final few things off.


# Why
There's plenty written on the benefits of continuous deployment (right?), but how do they specifically apply to our organisation?  Here are a few example of where I think we're currently being held back by *not* practising continuous deployment:

* currently, it often takes several weeks to realise the value of an idea into deployed, production software and often there's confusion over whether a particular feature is "done";
* a lack of time to spend on "innovating" (i.e. implementing those features which people *think* would be a good idea but there never seems to be time for); and
* it's common to have to manage backporting of critical bug fixes to already deployed versions (instead of just fixing on master and deploying)

# The Good...
## Continuous Integration
We practise [proper continuous integration](http://www.thoughtworks.com/continuous-integration).  Every change has to be submitted as a [github pull request](https://help.github.com/articles/using-pull-requests/), which is then reviewed, updated if necessary, and then merged in to master, as long as [travis](https://travis-ci.org/) is green (of course!).

We aim to keep PRs small, so that people are having their code merged in to master at least on a daily basis or so.  There is an understanding in the team that anything that goes in to master *could* become a release candidate so it has to work.

## Version Control Everything

In the last year or so, we've completed a migration to managing our infrastructure as code with [chef](https://www.chef.io/chef/).  We took this opportunity to get all of the developers on board with tools such as [vagrant](https://www.vagrantup.com/), so that we now have developers using production-like environments to do much of their work.

The upshot of this is that it releases are low ceremony events, with relatively few suprises when promoting appplications from development through to production.  There is now good collaboration between developers and operations to keep the infrastructure code working well for both teams.

Apart from putting all of our source code under version control, we've started to version control some other things in the last couple of years.

For example, we use [liquibase](http://www.liquibase.org/) to manage database migrations and all of this ends up in version control.  We even manage our [geoserver content in git](https://github.com/aodn/geoserver-config).  This last one has been really interesting.  There was initially a bit of resistance from content managers when we brought this in, as it seemed as they were being slowed down - having to check things in to git, submit PRs, wait for reviews etc.  What we actually find now is that many more people can contribute to geoserver content concurrently *and* it's now trivial to re-create test or production environments in a sand-boxed VM for development purposes, thereby speeding up content management and creation overall. Version control FTW!
 


# The Bad...

We use [jenkins](http://jenkins-ci.org/) to test and build our various products.  Although jenkins has in general served us quite well so far, I think we're reaching the limits of what it was designed particularly with respect to the more complicated build pipelines that we now have.

The other major issue we have with jenkins is the fact that it contains a lot of manual configuration - resulting in lots of duplication and slight inconsistencies where there shouldn't be any.  

I think we could benefit by addressing both of these issues, possibly by meta-generating job configs and/or moving to a more pipeline oriented tool such as [Go CD](http://www.thoughtworks.com/products/go-continuous-delivery).

		
		
# ...and The Ugly
- no automated acceptance tests
- no zero-down time releases
- co-ordinated releases for multiple services

# Before

	

# Future/Bad
* manual acceptance tests
* poor collaboration between developers and QA - ## DevOps Culture
* automated acceptance tests
	* a select set of well targetted happy-path use cases
	* maps - very visual - screen shots for a manual, but *fast*, test gate
* vagrant/development envs
* CI
	* generate jobs/pipelines using chef to achieve consistency, remove redudancy
	* GoCD?
* rollbacks/restores from backup
	