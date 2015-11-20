---
layout: post
title: Meteor - How to Create a Package
description: How to create a package and test application in Meteor
keywords: meteor, meteor package, meteor create package, meteor tutorial, meteor package tutorial, meteor how to create package
author: curtis_larson
comments: true
---

## Introduction

I usually always have to google the steps to create a Meteor package, so I figured I would do a write up for myself that I can reference. I hope this helps anyone else that has trouble creating a meteor package

## The Steps

### 1. Create the package


Open a terminal and navigate to the directory you want to store the package in. Type

    meteor create --package atmosphereusername:packagename

### 2. Create the test application

Enter the following commands. This will create a test app you can use to test your package.

    cd packagename
    meteor create test-app
    cd test-app

### 3. Create a packages folder for test-app

We need a place to store our new package in our test app

    mkdir packages

### 4. Symlink your package to the test-app

Now we can link our new package into our test-app, so that any modifications we make to the package will show up in our app.

    cd packages
    ln -s ../../../packagename ./packagename

### 5. Add the package

All that's left is to add the package to our test-app

    meteor add atmosphereusername:packagename


## Conclusion

That's it. Now when you run `meteor list` in your test-app you should see the packagename show up. If you are interested in wrapping a npm package in a Meteor package you can also check out my new tutorial, [Meteor - Wrap and Publish a NPM package](http://www.curtismlarson.com/blog/2015/11/20/meteor-wrap-publish-npm-package/). Now go write the next awesome meteor package!
