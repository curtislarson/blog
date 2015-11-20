---
layout: post
title: Meteor - Wrap and Publish a NPM package
description: How to create a smart package that wraps an npm package and publish it to atmosphere
keywords: meteor, meteor npm, meteor package, create meteor package, meteor npm package, meteor smart package, meteor create package, meteor atmosphere, meteor publish package, meteor wrap npm
author: curtis_larson
comments: true
---

## Introduction

Building off of my previous tutorial on [how to create a package in Meteor](www.curtismlarson.com/blog/2015/11/19/meteor-how-to-create-package/) I decided to create another tutorial on how to wrap and publish a NPM package. There are thousands of useful npm packages that can be used through `meteorhacks:npm`, but sometimes it's nice to create a meteor smart package and add some extra functionality to make it even easier to include in your project.

For this tutorial I will be wrapping the [Amazon Product Api](https://github.com/t3chnoboy/amazon-product-api) Node.js client. The full source code for the Meteor package is available on my GitHub [here](https://github.com/quackware/meteor-amazon-product-api).

## The Steps

### 1. Create a package

Similar to the [how to create a package](www.curtismlarson.com/blog/2015/11/19/meteor-how-to-create-package/) tutorial, we need to create a new package.

    meteor create --package atmosphereusername:packagename

### 2. Edit the package.js file

Now we need to make some edits to the `package.js` file to specify our Npm dependency and point the package to our server file where we will be making some Meteor specific customizations. First we add the following Npm dependency

    Npm.depends({
      "amazon-product-api": "0.3.5"
    });

And we also need to add an export in the `Package.onUse` block, we're going to choose `AmazonProductApi` as our export.

    Package.onUse(function(api) {
      api.versionsFrom('1.2.1');
      api.export("AmazonProductApi");
    });

Finally we need to reference the javascript file in `package.js` that will glue together the Npm package with Meteor.

    Package.onUse(function(api) {
      api.versionsFrom('1.2.1');
      api.export("AmazonProductApi");
      api.addFiles(["server/amazon-product-api.js"], "server");
    });

### 3. The Glue File

Go ahead and create the `amazon-product-api.js` file.

    mkdir server
    touch server/amazon-product-api.js

In this file we can add our `Npm.require` and setup the object we need to export.

    var amazon = Npm.require("amazon-product-api");

    AmazonProductApi = {};

Now the amazon product api package has three functions: `itemSearch`, `itemLookup`, and `browseNode` that are exposed in the `createClient` function. These functions have a future usage and a callback usage. Let's go ahead and add a syncronous usage to better use it in our Meteor projects. To do this we just need to add the `createClient` function to our exported object and wrap the functions using `Meteor.wrapAsync`. Below is the full code:

    var amazon = Npm.require("amazon-product-api");

    AmazonProductApi = {};

    AmazonProductApi.createClient = function(credentials) {
      var client = amazon.createClient(credentials);
      client.itemSearchSync = Meteor.wrapAsync(client.itemSearch);
      client.itemLookupSync = Meteor.wrapAsync(client.itemLookup);
      client.browseNodeSync = Meteor.wrapAsync(client.browseNode);
      return client;
    };

### 4. Publishing to Atmosphere

Now that our package is complete, we can publish it to atmosphere so that other people can use it. Make sure you are in your package directory and type the following command:

    meteor publish --create

Your package should now be built and uploaded to atmosphere.

## Conclusion

That's it! Now whenever someone wants to use the awesome [Amazon Product Api](https://github.com/t3chnoboy/amazon-product-api) npm package, they can just type `meteor add quackware:amazon-product-api`. They can also take advantage of our new syncronous functions we added. Again, the full source code for the Meteor package is available on my GitHub [here](https://github.com/quackware/meteor-amazon-product-api). Leave a message if you have any questions or comments!
