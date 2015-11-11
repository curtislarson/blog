---
layout: post
title: Meteor - iron:router Scroll to Top
description: How to scroll to the top of the page after every iron:router route.
keywords: meteor, meteor iron:router, meteor iron router, meteor iron router, iron router, iron:router scroll, scroll to top, meteor scroll, meteor scroll to top, meteor iron:router scroll to top, iron:router scroll to top
author: curtis_larson
comments: true
---

## Introduction

I use the awesome [iron:router](https://github.com/iron-meteor/iron-router) package in all my Meteor projects. One problem I run into in almost all packages is if you use `Router.go()` while scrolled down onto the page, your next page will rendered still scrolled at the same point. Here is some code that will fix that and scroll to the top of the page on each route.

## The Code

We are just going to use a simple `autorun` function that depends on the reactive `Router.current()` object. Check out the code below

<script src="https://gist.github.com/quackware/a66c39297edd0c139446.js"></script>

That's it! Very simple, but something I always include in my boilerplate code when I first create an application. Make sure to leave any questions or comments you have.