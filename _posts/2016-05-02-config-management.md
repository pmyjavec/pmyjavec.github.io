---
layout: post
title: "The Pitfalls of Configuration Management"
date:   2016-05-02 09:00:00
categories: code
---

It's almost unheard of for information technology organisations to be without a configuration management system of some sort;
however, it's not uncommon for these systems to be misused, misunderstood and therefore slowing an organisation down.

Considering configuration management systems are often at the core of how infrastructure is managed and heavily influence the way
organisations operate, healthy functioning is key to frequent delivery of quality products and reliable services.  Problems
begin to arise when these systems start to complicate the operations they're designed to simplify, thus
creating a negative return on investment.

Take Puppet for example, they proudly display the following message on the [front page](http://puppet.com):

> Are you spending more time fighting fires than deploying new, innovative services and applications? You are not alone.
> On top of that, most IT teams say it takes weeks, even months, to deploy critical applications. There’s got to be a
> better way...

While I understand and agree wholeheartedly with the message the above statement is trying to convey, it's so
easy for a poor implementation and understanding of Puppet to create the exact problems it's designed to solve.

After working with several different organisations and technologies I see the following erroneous patterns emerge
constantly:

* Incorrect usage: Is Hiera your database? Ansible your roster management system? Salt managing fine grained user access rules?
* Scaling, is your monolithic Puppet codebase used to solve all of your organisations problems?
* Incorrect / archaic technology, I bet you have two configuration management systems, one for orchestration.
* Poorly maintained code, spaghetti code will often equate to spaghetti systems.
* Configuration data becomes stale, huge, redundant and tightly coupled with the code, making changes difficult.
* Lack of continuous integration, staging environments and peer review.

Try look for these patterns in your own systems, can you identify them? If you can find some of these issues
and address them sooner rather than later, you will probably save yourself from some headaches and late nights.

Do bug fixes make up most commits in the configuration management codebase? Are peer reviews involved when changes
are made to improve communication and share knowledge? Do pipelines exist to automate deployment? Are the configuration
management systems solving the right problems? These are some of the important questions I use to help identify the issues outlined above.

It's now been over ten years since we've had some solid configuration management systems available. The time has come
to take stock and ask yourself whether these systems have been enabling or disabling your organisation.

If your configuration management systems aren't enabling your group by simplifying operations and helping you deliver
high quality products and services with less effort, then it's time to ask yourself, why not?
