---
layout: post
title: The Engineering Ice Cream
---

Problem Outline

The modern, cross-functional, software team is expected to not only write the software, but operate it in production. The old way of throwing our new features over the wall to a dedicated operations team to deploy and run is gone. There is plenty written about why this is, so I won't go in to it here (TODO: refs).


Detail

How do we factor operational concerns in to our daily workflow? The sorts of things I'm talking about:

* a bug has been released in to the wild, and we have several customers contact support about it because they can't complete their work because of our software
* we have a component that sometimes fails (these are the worst)
* we have an alerting system, but due to a flood, or lots of false positives... TODO

Customers are unhappy, developers are overloaded trying to balance development of new features with operating software.

Solution

Commonly accepted best practise for developing software is to practise Continuous Integration - whereby every code change is commited to a shared code reposiory, and subjected to a suite of automated tests. Teams who are doing CI well use the "Stop the Line manufacturing" technique - when a build fails, everyone stops what they're doing, and works together to address the failure immediately. This has the effect of continuously improving the process, and thereby over time, improving productivity.

Toyota - CI
"stop the line manufacturing" - we do this in work prior to release - well why shouldn't it be applied to production systems too?
ref: https://leanbuilds.wordpress.com/tag/stop-the-line/#:~:text=Stop%20the%20Line%20manufacturing%20is,defect%20on%20the%20assembly%20line.

To borrow a metaphor from CI, we have the "Testing Pyramid". The metaphor suggests that we get the most value from automated testing by focussing most of our efforts at lower layers (read: a large amount of automated unit tests) and less near the top (read: a small amount of manual exploratory testing). In contrast, Alister Scott talks about the "Software Testing Ice-Cream Cone" anti-pattern - this is where a team has fallen in to the trap of spending too much effort in manual regression testing - "chasing one's tail" as a former colleague put it.

In the context of developing and operating software, I think the ice cream cone is the correct "shape". At the top, we have the delicious creamy and sugary features that product managers, and ultimately customers, love and will keep coming back for. At the bottom, we have the production systems (need a better way to describe all of this), monitoring, etc that holds it together. Get a hole down there, and your delicious features melt and drip out through the bottom.

The shape and composition of the ice cream is correct: most of the effort goes in to the features at the top, with a much smaller amount going in that at the bottom of the cone. If something breaks at the bottom of the cone, you down tools and fix it straight away (Toyota's "Stop the Line").








Testing Pyramid - borrow from elsewhere


Solution

Metaphor borrowed from <a href="https://alisterbscott.com/kb/testing-pyramids/" target="_blank">Testing Pyramids & Ice-Cream Cones</a>
The engineering icecream:
- stop the line and jump on production issues straight away
- the premise is that these interruptions should be infrequent
- not doing this results in more work in the long run
- might be painful at first
