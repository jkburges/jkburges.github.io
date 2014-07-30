---
layout: post
title: On the road to Continuous Deployment at IMOS
comments: true
---

# Why
* currently takes a while to realise the value of an idea into software
* backporting critical bug fixes to deployed versions (instead of just fixing on master and deploying)
* more time to innovate (uptake of data, personal job satisfaction)



# Before

* puppet managed production envs
	* didn't include everything (e.g. webapp deployments)
	* limited usage amongst developers (e.g. to run up own VMs)
* YOLO style content management
    * one person responsible for configuring geoserver (bottleneck)
* No CI


# Current/Good

* pretty good devops culture
	* excellent collaboration between developers and ops
	* vagrant/virtualbox for production-like development/test environments (still some work to be done here) - regular usage
	* all code changes are reviewed via github PRs
	* good unit test coverage
	* understanding that if it goes in to master, it could be deployed and *should* be working (but still manual acceptance test phase)

* configuration management via chef
	* vagrant
	* code reviews
	* regularly executed, even by those not necessarily writing chef
	
* DB migrations (but generally can't roll-back)
	
* CI
	* jenkins
		* it works (most of the time), but messy config.  Needs more investment here
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
	