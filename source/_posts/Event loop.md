---
title: Event loop
date: 2023-12-21 10:12:30
tags:
---

## (In p33. The Event Loop in practice)

## Q: Isn't setTimeout(),setImmediate() has callback ?

Hi Jonas,

you said at 5.30 that there's nothing to do with event loop, but both have callback's and hence event loop should come in picture. do clarify.

## A: Here's the summarize

Only callback functions code INSIDE the poll phase code block(i.e. I/O loop phase), they will have execution order.

Else, they are still in event loop, but No execution order

![img](https://img-c.udemycdn.com/redactor/raw/q_and_a/2022-09-30_01-04-59-383d660944779bf346b4bea9acee0ced.png)



# Here's the details

The explanation in the video is **really** confusing and actually I was disappointed in discovering no ‘official’ answer has never been given, even though the question seemed to be shared among many users from many months (I guess years now).

I went through the documentation on my own and this page finally answered me:

https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/

Here the key points to note. I will use italic to citations from the official docs linked page:

\- Callbacks from setTimeout and setImmediate in the top-level code from the video DO ARE executed in the event loop. They are not top-level code. There is simply a caveat to understand why they are scheduled randomly, possibly breaking the event loop phases order. Keep reading for understanding why ;)

*-A timer specifies the threshold after which a provided callback may be executed rather than the exact time a person wants it to be executed. Timers callbacks will run as early as they can be scheduled after the specified amount of time has passed; however, Operating System scheduling or the running of other callbacks may delay them.* -> this means there is no way to know when timer callbacks will be scheduled in the timer queue. We only know after when they will be scheduled.

*- Technically, the* [*poll phase*](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/#poll) *controls when timers are executed.* -> this is a first hint that says: **without a scheduled I/O loop phase, there is no control on callback execution order.**

\- The poll phase is well explained [here](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/#poll) -> Technically speaking, this phase is the **heart of the event-loop** since it both monitors the presence of expired timers (i.e. timer callbacks scheduled in the callback queue) and of callbacks in the **check** phase (setImmediate callbacks). **It orchestrates the entire tick event loop.**

\- If there are setImmediate callbacks, the (emptied) poll phase stops and executes immediate callbacks, otherwise it moves on to timers

\- The poll phase is not therefore only executing callbacks, it also polls new callbacks and **idles whenever there are no pending callbacks in other phases**

*-Generally, as the code is executed, the event loop will eventually hit the poll phase where it will wait for an incoming connection, request, etc. However, if a callback has been scheduled with setImmediate() and the poll phase becomes idle, it will end and continue to the check phase rather than waiting for poll events*

We can summarise the entire behaviour by saying that the presence of a valid so called I/O cycle (i.e a valid phase of I/O polling with at least one valid callback to execute) is paramount in setting up an order of execution for callbacks in all event loop phases. **It is an entry point for callbacks execution order.** This leads to two consequences:

\1) setImmediate callbacks actually **precede** timer callbacks, since they come immediately after the I/O cycle, which synchronises all callbacks. Using a setImmediate callback ensures it will be executed **as soon as possible** (here the immediate attribute starts to make sense).

\2) **Without a synchronisation I/O phase, there is no actual starting point for callbacks synchronization**. The order of arrival of timeout/ immediate callbacks strictly depends on the process behaviour. This justifies what Jonas said in the video. setTimeout and setImmediate are indeed called one after another in the top level context, **without setting up any I/O callback**. Later on in the code, these instructions are then repeated in the file read callback, where therefore an I/O phase is fired. Since then, callbacks entry points are stated, and their execution order (first the setImmediate, BEFORE timers, then the timers) becomes deterministic.

A similar example as the one provided in this video is given here, from the linked page (screenshot for quick reference):



![img](https://img-c.udemycdn.com/redactor/raw/q_and_a/2021-09-12_14-45-48-5b1eced2b7673bee0de781ee6edf592d.png)

![image-20230930113001220](C:\Users\orang_9qy7n8e\AppData\Roaming\Typora\typora-user-images\image-20230930113001220.png)