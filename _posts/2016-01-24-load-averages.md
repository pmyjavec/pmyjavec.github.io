---
layout: post
title:  "Understanding Load Averages in Linux"
date:   2016-01-24 21:25:29
categories: linux
---

# Introduction

When working with load averages one of the most common misconceptions seems to be that "load averages" become high
when a system is super busy doing hard work.

Within Linux systems the load average metric is not an indication of how hard the CPU is working but how much work
it has to do.

When speculating about high load averages it's important to remember **load averages are the average number of *ready
to run / running* processes in the run queue.**

Think about a greenskeeper as a single core CPU. If he is mowing 1 lawn (processing) and has 5 more customers (processes)
waiting for their lawn to be mowed that day then his load average for the day is 6,
no matter how hard he is working to mow the current lawn.

*On a side note the greenskeeper is free to choose which lawn he mows and how much of it he mows at anytime based on
priority (this is kinda how the scheduler works)*

If we were to observe a load average of 5 on a 4 core machine, we can assume that at least one process will be put on
hold while the other 4 processes use up their alloted share of CPU time.

When dealing with applications in which waiting for CPU time is not a major issue (think batch processing), loading up
systems this way is common practice. On a web server it's desirable to leave a little headroom to account for traffic
spikes etc.

# Load averages can be deceiving

Let's say we have an already busy web server with a load average of around 4 on a 4 core machine. The load average is 4
because it's busy legitimately serving requests at a peak time of the day and it's configured to run a maximum of 4 web
server processes.

Then all of a sudden the SAN server hosting content up for web requests slows down dramatically.
Would our load average change? Not really, why? Remember the load average indicates the number of ready to run and
running processes, not how fast the HTTP requests are being served.

If one was just monitoring load averages they would probably miss a critical issue here. That is, customer's requests
have slowed to a crawl without a change in load average.  What would change would be the amount of time the server is
spending in *system* and *wait* time.

# Experiments

Checkout these experiments on an old dual core CPU with a 7200RPM ATA Disk:

*CPU under heavy computation workloads for ~2 minutes, load average is at ~2 after one minute:*

~~~
top - 18:38:18 up  9:48,  3 users,  load average: 2,07, 1,94, 1,60
Tasks: 164 total,   3 running, 161 sleeping,   0 stopped,   0 zombie
%Cpu0  : 99,3 us,  0,5 sy,  0,2 ni,  0,0 id,  0,0 wa,  0,0 hi,  0,0 si,  0,0 st
%Cpu1  : 98,2 us,  1,7 sy,  0,1 ni,  0,0 id,  0,0 wa,  0,0 hi,  0,0 si,  0,0 st

  PID  PR S  %CPU     TIME+ P COMMAND
27101  25 R   0,2   0:07.97 1 top
27859  20 R  90,9   1:43.14 1 bash -c while true; do (( 100 * 6 )); done
27862  20 R  83,1   1:18.71 0 bash -c while true; do (( 100 * 6 )); done
~~~

*CPU under heavy computation workloads with a saturated disk for ~2 minutes, note the 1 minute load average at ~2:*

~~~
top - 18:33:43 up  9:44,  3 users,  load average: 2,08, 1,97, 1,47
Tasks: 165 total,   2 running, 163 sleeping,   0 stopped,   0 zombie
%Cpu0  : 96,3 us,  0,0 sy,  0,0 ni,  0,0 id,  0,0 wa,  0,0 hi,  3,7 si,  0,0 st
%Cpu1  : 15,2 us, 32,5 sy,  0,4 ni,  0,0 id, *50,7 wa*,  0,0 hi,  1,2 si,  0,0 st

  PID  PR S  %CPU     TIME+ P COMMAND
27833  20 D  22,0   0:21.72 1 dd if=/dev/sda of=/dev/null bs=2MB
27101  25 R   0,1   0:07.37 1 top
27835  20 R  99,7   1:38.32 0 bash -c while true; do (( 100 * 6 )); done
~~~

*Lastly a classic example, high load averages with near idle CPU due to an unreachable synchronous NFS mount, perfectly
demonstrating load averages are about work the system needs to do and little to do about how hard it's working,
load average is almost ~2:*

~~~
top - 18:07:09 up 23 min,  1 user,  load average: 1,75, 0,71, 0,36
Tasks: 120 total,   2 running, 118 sleeping,   0 stopped,   0 zombie
%Cpu0  :  0,1 us,  0,0 sy,  0,0 ni, 99,8 id,  0,0 wa,  0,0 hi,  0,0 si,  0,1 st
KiB Mem :  1015472 total,   617456 free,    92368 used,   305648 buff/cache
KiB Swap:  2097148 total,  2097148 free,        0 used.   762476 avail Mem

  PID  PR S %CPU     TIME+ P COMMAND
 1680  20 D  0,0   0:00.00 0 touch /mnt/1
 1681  20 D  0,0   0:00.00 0 touch /mnt/2
   25  20 R  0,0   0:00.80 0 [rcuos/0]
 1682  20 R  0,2   0:00.11 0 top
~~~
