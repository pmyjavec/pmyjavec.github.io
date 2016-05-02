---
layout: post
title: "Configuration Management, No Problem"
date:   2016-05-02 09:00:00
categories: code
---

It's almost unheard of for information technology organisations to be without configuration management of some sort; However, it's not
uncommon for configuration management to be misused, misunderstood and therefore broken and slowing an organisation down.

Considering configuration management is often at the core of how infrastructure is managed and heavily influences the way
organisations operate, healthy functioning is key to smooth running systems and frequently delivery of quality product.

Configuration management goes bad when the configuration management system itself becomes the problem which needs to
be solved. That is, you're organisation is spending significant time making configuration management systems work for you.

Take Puppet for example, they proudly display the following message on the [front page](http://puppet.com):

> Are you spending more time fighting fires than deploying new, innovative services and applications? You are not alone.
> On top of that, most IT teams say it takes weeks, even months, to deploy critical applications. There’s got to be a
> better way...

While I understand and agree wholeheartedly with the message the above statement is trying to convey, it's so
easy for a poor implementation and understanding of Puppet to create the exactly problems it's designed to solve.

After working with several different organisations and technologies I see the following erroneous patterns emerge
constantly:

* Incorrect usage: Is Hiera your database? Ansible your roster management system? Salt for fine grained permissions control (no pun intended).
* Scaling: Is you're 10 year old monolithic Puppet codebase till trying to solve every groups in your organisations problems?
* Incorrect technology choice: Did you need built-in orchestration capabilities but chose Puppet and now are managing two systems?
* Code rot: Infrastructure as code is code after all, if it's not well looked after it rots.
* Configuration data becomes stale, redundant and tightly coupled with the code.

I often look for the symptoms above in systems I'm working with, especially when starting out in a new company, I then use the criteria
as a sort of _litmus test_ which then gives me an idea of what soft of problems I can expect when working with
environments they're used to manage. Most of the time, these symptoms have a root cause, they're cultural, surprised?

It's now been over ten years since we've had some solid configuration management systems available to us.  The time has come
to take stock and ask yourself whether these systems have been enabling your organisation or disabling your organisation?

Installing these systems are one thing, making them a competitive advantage and useful tool is another.

If these systems aren't adding immense value, then why not?
