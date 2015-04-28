---
layout: post
title: Meteor: How to Trigger a Function when a Session Value Changes
description: Meteor: How to Trigger a Function when a Session Value Changes
keywords: meteor, session trigger, session, meteor trigger session
author: curtis_larson
---

## How to Trigger a Function when a Session Value Changes

While working on a Meteor project, I wanted to trigger a function whenever I changed the value of a Session variable through `Session.set()`. It took me a little bit of experimentation and digging to figure out how to do it, and the result is pretty simple.

## How to do it

Use the following code:

    var doSomething = function() {
      // Do something when the session value changes
    }

    Deps.autorun(function() {
      var sessionVal = Session.get("yourSessionVariable");
      console.log("The session value has changed");
      doSomething();
    });

    var anotherFunction = function() {
      Session.set("yourSessionVariable", "foo");
    }

That's it! Whenever `anotherFunction` is executed, or whenever the session value `yourSessionVariable` is changed in any other function, the function in the `Deps.autorun()` block will execute. Simple!

## Deps.autoRun

`Deps.autoRun()` is the key here, as it can make arbitrary blocks of code reactive. `Session` is already a reactive variable, so we do not need to go through the trouble of using `Deps.Dependency`. You can find more information about Deps [here](https://manual.meteor.com/#deps).