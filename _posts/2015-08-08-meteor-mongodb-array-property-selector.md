---
layout: post
title: Meteor - MongoDB Object Array Property Selector
description: How to select a document from a collection based on a property in an array of objects.
keywords: mongodb, meteor, meteor mongodb, mongo, meteor mongodb property, array property selector, array, property, mongodb collection, collection, meteor collection
author: curtis_larson
comments: true
---

## Introduction

One slightly confusing question that I get asked sometimes and I see around the internet is how to select a document from a collection based on a property in an array of objects.

Suppose we have a `SimpleSchema` like so:

    Schema = {};

    Schema.Task = new SimpleSchema({
      name: {
        type: String,
        label: "Task Name"
      },
      tags: {
        type: [Object],
        label: "Tags",
      },
      "tags.$.name": {
        type: String
      },
      "tags.$.rank": {
        type: Number
      }
    });

    Tasks.attachSchema(Schema.Task);

And we have inserted the following document:

    Tasks.insert({
      name: "Write Meteor Tutorial",
      tags: [
        {
          "name": "Meteor",
          "rank": 0
        },
        {
          "name": "Tutorial",
          "rank": 1
        }
      ]
    });

And we want to select all the tasks that have a tag with the name "Meteor". We can use the following syntax to perform this task:

    Tasks.find({
      "tags.name": "Meteor"
    });

Notice how we did not include the `$` symbol that we used when specifying our `SimpleSchema`. Additionally if we only want to select the tags and not the entire `Tasks` object we can use a `fields` option.

    Tasks.find({
      "tags.name": "Meteor"
    }, {
      fields: { "tags.$": 1}
    });

The differing syntax may seem a bit confusing, but once you get the hang of it you can create some pretty powerful and complex schemas and selectors. Hope this helps anyone running into this problem!