---
layout: post
title: Controlled Chaos
comments: true
---

# Chaos

We have a situation [here](http://imos.org.au/emii.html) where we would like to allow those with the necessary skills, but who may not be full-blown developers, to experiment with and make changes to our (PostgreSQL) database.

In the past, this has been done by having a shared "development" DB, with any changes made there manually "migrated" to the production DB in a very ad-hoc manner.

This is problematic for a few reasons, but mainly because changes to the production server are unmanaged, and hence, its current state is fairly chaotic.


# Control

Ideally, we want people to have the ability to do their (development) work *using the tools that they are already familiar with*, but at the same time, provide an easy and well managed path to get their changes from development to production.


# Controlled Chaos

I am going to propose a workflow which I believe can give us *control* while still maintaining a (good) level of *chaos!*

You will need:

* a unixy OS, e.g. linux or OS X
* [virtualbox](https://www.virtualbox.org/)
* [vagrant](http://www.vagrantup.com/)
* [vagrant-berkshelf plugin](https://github.com/riotgames/vagrant-berkshelf)
* [git](http://git-scm.com/)

So let's clone the [associated github project](https://github.com/jkburges/emii_toolkit.git) and fire up vagrant:

```
$ git clone https://github.com/jkburges/emii_toolkit.git
$ cd emii_toolkit/
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
[default] Importing base box 'ubuntu-12.04-omnibus-chef'...
.
<snip>
.
[2013-09-18T05:48:26+00:00] INFO: Chef Run complete in 454.580271656 seconds
[2013-09-18T05:48:26+00:00] INFO: Running report handlers
[2013-09-18T05:48:26+00:00] INFO: Report handlers complete
$
```

Success (I hope)!

The code (and the VM) is all there for you to take a look at if you're interested, so I am not going to go in to *how* this all works.  Instead, let's go straight to an…

# Example Workflow

Let's say one of my colleagues would like to add a new table to the database, using his or her tool of choice, [pgAdmin](http://www.pgadmin.org/).  Here's how to do it:


1. Create a connection to the database residing within the VM (noting that the port is **15432** and password is **postgres**):<br/>
![new database connection](/assets/controlled_chaos/new_connection.png)<br/><br/>
2. Create the new table under **localhost VM/geoserver/public/tables**:<br/>
![new database connection](/assets/controlled_chaos/new_table.png)<br/><br/>

3. Run the diff script:<br/>
```$ bin/diffChangeLog.sh great_new_table```

The way I've set this up is to have a [*different* git project](https://github.com/jkburges/emii_db) for the DB migrations (to keep them separate from all the vagrant/environment infrastructure).  However, it lives within our current hierarchy, and we can go there to see what changes have been made to *that* repo:

```
$ cd workspace/emii_db/
$ git status
#
#	modified:   changelog.xml
#
# Untracked files:
#
#	great_new_table.changelog.xml
```

What we have here are a number of [liquibase](http://www.liquibase.org/) changeset files - `changelog.xml` being the main one and `great_new_table.changelog.xml` being the one just generated and referenced from the former.

Without going in to details about liquibase, we can see that our changes (made through our favourite tool) have been transformed in to an executable format, belonging (almost) to a git repository.

At this point, what happens depends on your organisation's workflow.  At [eMII](http://imos.org.au/emii.html), we would push the changes to a git branch, submit a pull request, have it reviewed, before being merged back in to `master` and finally deployed to production.

# Summary

I really haven't gone in to much detail about how this is all actually implemented.  Really what I want people to take out of this post is that it's possible to have a process which allows people (who are not necessarily developers by trade) to get their changes in to a production database, but in a controlled manner.

Comments, as well as pull requests to the associated github project, are welcome!
