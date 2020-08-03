---
layout: post
title: The Engineering Ice Cream
---

Problem Outline

The modern, cross-functional, software team is expected to not only write software, but also to operate it in production. The old school way of throwing features over the wall to a separate operations team to deploy and run is gone. There is plenty written about why this is, so I won't go in to it here (TODO: refs).


Detail

Now that engineering teams have these extra operational concerns, how do we factor them into our daily workflow? The sorts of things I'm talking about might be:

* a bug has been released into the wild, and we have had several customers contact support about it because they can't complete their work;
* we have a component that usually works but sometimes fails (these are the worst);
* our monitoring system has triggered an alert, but we are not sure whether it's a real problem or not

Customers are unhappy, and engineers are overloaded trying to balance development of new features with keeping things running in production.

# Building software

Building software nowadays, it's commonly accepted to practise continuous integration (CI): every code change is commited to a shared repository against which a suite of automated tests is run (called a "build"). If a build fails, it is fixed straight away, and importantly, _before_ any other code changes are made. It quickly gets the code back to a known good state rather than building on something broken, which could be harder to rectify.

It's an idea that builds on the concept of _kaizen_:

> The Toyota Production System is known for kaizen, where all line personnel are expected to stop their moving production line in case of any abnormality and, along with their supervisor, suggest an improvement to resolve the abnormality which may initiate a kaizen.

\- [https://en.wikipedia.org/wiki/Kaizen](https://en.wikipedia.org/wiki/Kaizen)

# Testing software

A metaphor used in CI is the "Testing Pyramid". The metaphor suggests that we get the greatest value from testing by focussing most of our efforts at lower layers (many unit tests), the least at the top (a few end-to-end (E2E) tests), and a moderate effort in the middle (integration tests).

<a title="Abbe98 / CC BY-SA (https://creativecommons.org/licenses/by-sa/4.0)" href="https://commons.wikimedia.org/wiki/File:Testing_Pyramid.svg"><img alt="Testing Pyramid" src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/64/Testing_Pyramid.svg/512px-Testing_Pyramid.svg.png"></a>

In contrast, Alister Scott talks about the [Software Testing Ice-Cream Cone](https://alisterbscott.com/kb/testing-pyramids) anti-pattern, where a team has fallen into the trap of spending too much effort in manual regression testing and hence the shape is more like an ice-cream cone than a pyramids.

![link to image if permission](https://alisterbscott.com/wp-content/uploads/2018/02/software-testing-icecream-cone-antipattern.jpg)

# Operating software

Where I am going with all this is that for _operating_ software, I think an ice-cream cone is desirable.

[insert diagram here]

At the top, we have the delicious creamy and sugary features that product managers, and ultimately customers, love and will keep coming back for. At the bottom, we have the production infrastructure - servers, monitoring, alerting and logging systems - that hold it all together. Get a hole down there, and your delicious features melt and drip out through the bottom. The shape is right because we want to be putting most of our effort in to features, and less of our effort in to operations.
