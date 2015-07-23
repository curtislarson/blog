---
layout: post
title: Meteor - How to use iron:router waitOn with an Async Server Call
description: How to use iron:router waitOn with an async server call (Meteor.call)
keywords: meteor, iron router, iron, iron router waitOn, waitOn Meteor.call, waitOn async
author: curtis_larson
comments: true
---

## How to use iron:router waitOn with an Async Server Call

There have been several [posts](http://stackoverflow.com/questions/29493809/how-to-make-onbeforeaction-call-wait-until-a-function-call-inside-finishes-in-me/) about utilizing iron:router's `waitOn` functionality for an async server call. After being disatisfied with a lot of the solutions, including my own in the aforementioned stackoverflow post, I decided to write up a simple solution that had a minimal amount of extra code in the routes file. A couple of problems that I (and many others) ran into included the `waitOn` function running in an [infinite loop](http://stackoverflow.com/questions/25136239/meteor-0-8-3-iron-router-infinite-loop-inside-waiton-hook), and also `waitOn` [executing twice](https://github.com/iron-meteor/iron-router/issues/1031).

## Client Routes File

The client routes.js file is very simple, as the core functionality is contained in a different file. Basically what I am doing is returning a `Util.waitOnServer` call which will call the `testWaitOn` Meteor method on the server. You can optionally pass in a second argument to `Util.waitOnServer` to pass data to the Meteor method. Once this call returns, we can then access the data through `Util.getResponse`.

<script src="https://gist.github.com/quackware/71290757e28d7df89540.js"></script>

## Util.waitOnServer

The Util object contains all the functionality of waiting on `Meteor.call`. Functionality such as `getResponse` can be easily changed to something more eloquent, and you can also change the `Meteor.call` to be any async call. The `data` arugment from `Util.waitOnServer` is forwarded to the Meteor method here.

<script src="https://gist.github.com/quackware/eea7818fde0cda6a35c3.js"></script>

## Server Code

The server code is very simple, it just generates a random number and returns it. I also printed out the `data` argument passed to us from the client. I just wanted a way to make sure everything was being executed in the correct order.

<script src="https://gist.github.com/quackware/703e6cf54c24f7f9b16f.js"></script>

The full source code is available [on my GitHub page](https://github.com/quackware/meteor-waitOnServer). If you have any questions please post them in the comments!