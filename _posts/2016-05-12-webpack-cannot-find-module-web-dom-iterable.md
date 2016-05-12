---
layout: post
title: Webpack - ERROR in Cannot find module '../modules/web.dom.iterable' @ multi main
description: How to fix ERROR in Cannot find module '../modules/web.dom.iterable' @ multi main error
keywords: webpack, webpack modules, node, node webpack, webpack web.dom.iterable, web.dom.iterable, multi main error, webpack multi main
author: curtis_larson
comments: true
---

## The error

I started up a new webpack/react project recently and immediately ran into this error:

    ERROR in Cannot find module '../modules/web.dom.iterable'
     @ multi main

After reinstalling `node`, `npm`, adding various modules such as `core-js` I was still unable to fix the problem. The solution that finally fixed it was actually the simplest

    rm -rf node_modules && npm install

After I completely reinstalled my local `node_modules`, everything worked! I hope this saves someone else a few hours of time.
