---
layout: post
title: Meteor 1.3+ - Connect to Redis on the Server
keywords: meteor, meteor redis, redis, meteor connect to redis, meteor redis server, apollo, meteor apollo, meteor apollo redis, meteor 1.3, meteor 1.3 redis, node redis, node redis tutorial, meteor redis tutorial, redis tutorial
author: curtis_larson
comments: true
---

## Introduction

While doing research on backend connectors for the new [Apollo Stack](http://www.apollostack.com) by the Meteor team I wanted to easily connect to a redis server from my own Meteor server. The following is a writeup of the steps to set this up. As always the full source code for this tutorial is available on [my GitHub](https://github.com/quackware/meteor-redis-tutorial).

## Install Redis

We can download redis onto your computer from their [download page](http://redis.io/download). Mac users that have homebrew can also easily install it via `brew install redis`. The rest of this tutorial will assume you are running redis on a Mac.

Once redis is installed you can start it by just typing `redis-server` which should give you the following prompt:

![Redis Prompt](http://i.imgur.com/KvxiYaw.png)

Notice that it is using the default port of `6379`. If you want to change this port from the default you will also need to change the way your redis package connects to the server. You can also play around with the server by typing `redis-cli` and manually enter in commands in the cli.


## Install Packages

We also need to install the following packages to get redis setup. The most notable is [node_redis](https://github.com/NodeRedis/node_redis) which allows us to communicate with the redis server we installed in the previous step.

    meteor npm install --save redis
    meteor npm install --save hiredis

## Server Side Redis

Setting up redis on the server is fairly simple, as you just need the following code:

<script src="https://gist.github.com/quackware/2b06110fa475ac6080a3f5f1c69e7789.js"></script>

We require the redis package, create a client from it, and wrap the `get` and `set` functions so that they can easily be used with `Meteor.methods`. We then export the client so it can be used in any other modules by just calling `require("redis.js")`. If you want to change the default port you can pass in additional options to `createClient`. A full overview of the api is available on the [node_redis GitHub page](https://github.com/NodeRedis/node_redis).

## Meteor Methods

The meteor methods are very basic for testing purposes. We just have a simple `setRedis` and `getRedis` method that use our wrapped redis functions. This allows us to test the redis functionality from the client.

<script src="https://gist.github.com/quackware/42058d667822fcd7cf4e5c39ddafcf45.js"></script>

## Client Side Code

The client side code is composed of a few input fields and buttons that are hooked up to `Meteor.call` code. This allows us to test our simple `get` and `set` functions.

<script src="https://gist.github.com/quackware/2b9919c242942ee71af74e6f312e8a41.js"></script>

<script src="https://gist.github.com/quackware/4b3ba350013f346fddd38460a3db8f5f.js"></script>

Clicking `set` will add the key/value pair to the redis server. You can then retrieve the value by clicking `get` which will populate the `Get Value` label.

## Conclusion

That's it for this short tutorial with the bulk of the content being installing and setting up redis on the server. As you can see it's very straightforward to add data stores other than MongoDB to your Meteor application, all thanks to the new npm support provided in Meteor 1.3+. As always the full source code for this tutorial is available on [my GitHub](https://github.com/quackware/meteor-redis-tutorial).

