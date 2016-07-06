---
layout: post
title: Webpack & Heroku - Build failure with no error message
description: A potential fix to a build failure with no error message when you use webpack and heroku.
keywords: webpack, webpack heroku, heroku, heroku node, webpack node, webpack node heroku, webpack build error, heroku error webpack, heroku webpack failure no error, webpack failure, webpack heroku failure
author: curtis_larson
comments: true
---

## The error

Another fix for a webpack build error here! This one took a little while to figure out, and is directly related to the heroku deploy system. The symptoms are an unexpected failure in the heroku build process, often with the last log message being something like:

     68% 834/858 buil

## The Fix

In my `webpack.config` I had my `deploy` script as follows:

    "webpack --config --progress webpack.production.config.js"

Removing the `--progress` flag ended up fixing the problem. The underlying issue was that the large amount of log / print statements that `--progress` was generating was causing the build process to crash on heroku (but not locally). I hope this saves time for anyone else with this issue!