---
layout: post
title: Ops Before Apps
---

# Works for me!
We've all spent plenty of time chasing down issues related to non-standard environments - different DBs, different containers and not to mention different OSs - there can be subtle (or not so subtle) differences that can cause us headaches.  The earlier we find these differences out - the better.  The less permutionations of the above that we have to deal with - even better.

# Ops before (or is it with) apps?

Through the magic of chef, berkshelf, vagrant and jenkins, I'm going to present to you a method for minimising the types of 'environmental' problems that you might normally run in to. I say that when starting a new project, 'operations' code should be written before any application code (importantly, I include in 'operations' configuration of the continuous integration server).

Why?  Well, because:

1.    operations code is going to have to be written eventually anyway;
2.    continuous integration for the project will have to be configured eventually;
3.    we get the benefits of consistent and standardised environments for development, testing and production;
4.    all of the above are much easier to get going *early* in a project's life.

# Getting Started

*Note: this post and associated github project utilise git, chef, vagrant, jenkins and grails.  This is in no way intended to be a tutorial on these technologies - that is what google is for.*

There's a [github project](https://github.com/jkburges/ops-before-apps) to go along with this post, so let's get going with it:

    $ git clone https://github.com/jkburges/ops-before-apps.git
    $ git checkout getting_started
    $ vagrant up

Now, this takes a while (about half an hour on my machine).  Yes, it's a long time to wait to see the magic, but hopefully it's worth it!

After the above has completed, we should have three VMs (let's hope you've got enough RAM!):

## Jenkins
Navigate to the [jenkins instance](http://192.168.50.2:8080) and note that there's a *passing* job configured for our wonderapp, 'not-another-bookshop'.  Note that this VM is included only for the purposes of providing a fully self-contained demonstration. In reality you would already have a CI server (be it jenkins or otherwise) and would use that.  The important thing to note about it really is that it was the provisioning of the "dev" VM that actually configured the job on jenkins (including required plugins).

## Dev
This is the VM where, surprisingly, you're going to be doing all of your development work (sort of - as you already know if you've used vagrant before, the `/vagrant` directory is shared between the guest VM and the host.  This allows you to code in your host OS and favourite IDE, pretty important I think to keep developers happy).

Let's ssh in and start the grails app:

# TODO: check this, provide some output.
    $ vagrant ssh dev
    $ cd /vagrant
    $ grails run-app

Navigate to [not-another-bookshop (development)](http://192.168.50.3:8080/not-another-bookshop) and you will see a *very* barebones grails app.  This was created in the usual grails manner, i.e. `grails create-app not-another-bookshop` , with no code having been written by me (yet).  The important thing is that we have a working dev environment and our app runs in it (in the grails development container).

## Prod

Navigate to [not-another-bookshop (production)](http://192.168.50.3:8080/not-another-bookshop) and you will see very much the same thing as with [dev](#dev), except that this instance is running in a production like environment.  That is, the war file is being pulled from jenkins (by chef) and being deployed in an actual tomcat container.  Really, we should be able to take this chef config and apply it against a *real* node, when the time comes.

# Books, books, books

Ok, now we've got all this in place, let's actually create some application code.
TODO: do this - with integration test.

TODO: git tag

# PostgreSQL FTW

Let's assume that we've written *quite a lot* more code than the above, and that for whatever reason, we've decided we better start using a bit more industrial strength DB to support our app.  This is an example where I believe the real power of this approach comes in.  Normally, each developer would go through their own process for installing PostgreSQL (or whatever the DB of choice is) on their own box.  *Then*, the ops team would go through another process (hopefully at least using chef, puppet etc) to do the same.  Here, we cut out a lot of work (and risk) by all using the same process/chef code.

Let's do it.


TODO: git tag


# Where to from here?

knife app create - generates a lot of the chef code, with perhaps some blanks to fill in
please submit pull requests if you find a better way of doing something!
