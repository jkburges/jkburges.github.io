---
layout: post
title: Ops Before Apps
comments: true
---

# Works for me!
We've all spent plenty of time chasing down issues related to non-standard environments - different DBs, different containers and not to mention different OSs - there can be subtle (or not so subtle) differences that can cause us headaches.  The earlier we find these differences - the better.  The less permutionations of the above that we have to deal with - more the better.

# Ops before apps?

Through the magic of chef, berkshelf, vagrant and jenkins, I'm going to present to you a setup for minimising the types of 'environmental' problems that you might normally run in to. I say that when starting a new project, 'operations' code should be written before *any* application code (importantly, I include in 'operations' the configuration of the continuous integration server).

Why?  Well, because:

1. operations code is going to have to be written eventually anyway;
2. continuous integration for the project will have to be configured eventually; and
3. we get the benefits of consistent and standardised environments for development, testing and production

All of the above are much easier to get going *early* in a project's life.

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
This is the VM where, surprisingly, you're going to be doing all of your development work (sort of - as you already know if you've used vagrant before, the `/vagrant` directory is shared between the guest VM and the host.  This allows you to code in your host OS and favourite IDE, pretty important I think to keep the developers happy).

Let's ssh in and start the grails app:

    $ vagrant ssh dev
    $ cd /vagrant
    $ grails run-app

Navigate to [not-another-bookshop (development)](http://192.168.50.3:8080/not-another-bookshop) and you will see a *very* barebones grails app.  This was created in the usual grails manner, i.e. `grails create-app not-another-bookshop` , with no code having actually been written (only generated).  The important thing is that we have a working dev environment and our app runs in it (in the grails development container).

## Prod

Navigate to [not-another-bookshop (production)](http://192.168.50.3:8080/not-another-bookshop) and you will see very much the same thing as with [dev](#dev), except that this instance is running in a production like environment.  That is, the war file is being pulled from jenkins (by chef) and being deployed in an actual tomcat container.  Really, we should be able to take this chef config and apply it against a *real* node, when the time comes.


# Summary

Ok, so all we've really done here is set up a bit of infrastructure for our app.  It's not rocket science.  Really it's the order of doing things that is changed compared to what most people seem to do.

I think where the power of this approach *really* comes in is when you want to change the infrastructure that your app relies upon.  For example, changing the DB to say PostgreSQL.  You are going to know straight away if there are any problems (there will be).  Any problems that do come up should be easier to fix now rather than later - and they will only have to be fixed once - rather than for each developer/OS combination, and then *again* for production.

Additionally, the development and ops teams will be on the same page from the get-go as to what your application's stack looks like, which can only be a good thing.

But what I'm really interested in are what other people's thoughts are on this approach - do you agree or disagree with me?
