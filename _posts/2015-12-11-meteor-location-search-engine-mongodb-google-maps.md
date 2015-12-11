---
layout: post
title: Meteor - Reactive Location Search Engine with MongoDB and Google Maps
description: How to create a reactive location based search engine in Meteor using MongoDB and Google Maps
keywords: meteor, meteor mongodb, meteor mongodb location, meteor location search, mongodb location search, meteor google maps, google maps meteor, meteor google maps search, meteor location search google maps, meteor reactive, meteor reactive google maps, meteor reactive maps, google maps reactive search
author: curtis_larson
comments: true
---

## Introduction

One cool feature that I have been adding a lot recently to my client's websites is the ability to search for jobs, users, etc and have the search results display reactively in Google Maps. This is relatively easy to accomplish in Meteor if we combine a few useful packages and use MongoDB's location searching functionality. In this tutorial we are going to create a simple website that allows us to add jobs with a title and location and then search for those jobs and have them display on an embedded Google Map. The full source code for this tutorial is available on [My GitHub Page](https://github.com/quackware/meteor-location-search). Let's get started!

## Dependencies

This tutorial requires quite a few Meteor packages that make our life easier. Below are the required ones that I will be referencing throughout the tutorial:

    meteor add aldeed:autoform
    meteor add aldeed:collection2
    meteor add dburles:google-maps
    meteor add ejson
    meteor add iron:router
    meteor add lukemadera:autoform-googleplace

You can also include these optional dependencies. They are not 100% necessary to get your app working, but make your life easier and I will be using them throughout this tutorial.

    meteor add less
    meteor add nemo64:bootstrap
    meteor add reactive-var

[aldeed:autoform](https://github.com/aldeed/meteor-autoform) and [aldeed:collection2](https://github.com/aldeed/meteor-collection2) are used primarily to make it easy to create forms on the client and submit data to the server. Additionally they are required if we want to take advantage of the awesome [lukemadera:autoform-googleplace](https://github.com/lukemadera/meteor-autoform-googleplace/) package which adds a Google places search input for autoform. autoform-googleplace requires `ejson` so we need to add that package also. [dburles:google-maps](https://github.com/dburles/meteor-google-maps) is a great wrapper of the [Google Maps JavaScript API](https://developers.google.com/maps/documentation/javascript/tutorial) that allows us to embed a live Google Maps display in our website. The final required dependency is [iron:router](https://github.com/iron-meteor/iron-router) which allows us to set up our routes and configure when we want to load in the Google Maps code.

The optional dependencies are `bootstrap`, `less`, and `reactive-var` which allow us to style our website very easily and take advantage of `ReactiveVars` so we can organize our JavaScript code in a clean manner.

## Additional Setup

Besides the various packages we need to install we also need to obtain a [Google Maps Key](https://developers.google.com/maps/signup?hl=en) that has been enabled for the `Google Maps JavaScript API` and `Google Places API Web Service`. This will allow us to access Google's apis for location search and map embedding. You can setup your key in your settings.json file like below:

<script src="https://gist.github.com/quackware/67fe8dcc60d9286f30e4.js"></script>

## Setting up our Database Schema

The first thing we need to do is setup our MongoDB and AutoForm schemas so that we can create our AutoForms and do location searches. The docs for [lukemadera:autoform-googleplace](https://github.com/lukemadera/meteor-autoform-googleplace/) show the structure of the Google Places data that is returned from the search input, so we can use that to create our `Address` schema. Additionally we are going to create a `Job` schema that will hold our job title and the location of the job. Finally we need to create a `Search` schema for our search autoform. You can view the code for all the schemas below (Note: All this code should be included in a lib/ folder so it can be accessed from both the client and the server):

<script src="https://gist.github.com/quackware/3d70789fa05556b80cec.js"></script>

Some important things to note include the `geometry` object in our `Address` schema. This object will hold the geometry type and coordinates (lat and lon) that will be used by MongoDB for location searches. We also define our `Jobs` collection and attach it to our schema. This is the collection we will be inserting job data into and searching for jobs with.

One additional step to ensure our schema works correctly with MongoDB location search is to define the correct index on our geometry data. This can be accomplished with an `_ensureIndex` call like below:

<script src="https://gist.github.com/quackware/0a3e7750b74e21d5ad56.js"></script>

## Publisher Code

Now that we have correctly defined our schemas we can implement the code that will perform the location search. We need to create a simple publisher that recieves search data from the client and uses a [MongoDB $near query](https://docs.mongodb.org/v3.0/reference/operator/query/near/) on our `geometry` object we mentioned above. You can see the code for the publisher below (Note: This code should be included in the server/ folder):

<script src="https://gist.github.com/quackware/667f8619b1dae4c99eed.js"></script>

`searchData` just contains the data we defined in our `Schemas.Search` SimpleSchema. Now whenever we subscribe to our `jobSearch` publisher with correct data we will receive location based results.

## Database Insert Code

Thanks to the simplicity of AutoForm, our code for inserting a new job into the database is very simple (Note: This code should be included in the server/ folder).

<script src="https://gist.github.com/quackware/f701543e946f40155a44.js"></script>

All we need to do is check the data passed into our `Meteor.method` with our `Schemas.Job` we defined above. If that data passes the check then we insert it into our `Jobs` collection so it can be searched for.

## Client Side Route

Now that our server side code is complete, we need to add the client side code that will send job and location information to the server. To start out, we need to create a simple route in `iron:router` that will load our `index` template. Additionally we want to make sure that we load the `GoogleMaps` api exposed by [dburles:google-maps](https://github.com/dburles/meteor-google-maps) so that we can render an embeded map in our template (Note: This code and the rest of the code in the tutorial should be included in the client/ folder).

<script src="https://gist.github.com/quackware/742264d5b0f8644e7dcd.js"></script>

The route code is pretty straightforward. The most complicated part is the `onBeforeAction` hook where we check to see if the `GoogleMaps` api has already been loaded, and if it has not then we load it using our Google maps api key we defined in our `settings.json` file. Additionally make sure to specify the correct `geometry` and `places` library so that we can both search for locations and embed our Google Map.

## Client Side Templates (Add Job)

Our client side html template will be composed of four parts. The first part is a simple AutoForm that allows us to submit a job title and location to the database.

<script src="https://gist.github.com/quackware/2e9316fc2b2a6f9b8fa5.js"></script>

The first thing to notice is our AutoForm schema `jobSchema` which is just a helper that returns `Schemas.Job`

<script src="https://gist.github.com/quackware/878c1fa83be337bcd714.js"></script>

This will allow us to specify our `title` and `location` quickfields. Also notice the `addJob` meteormethod which maps to our `addJob` database insert code we defined above. No extra code is needed to connect this form to that Meteor method, it's all handled automatically by AutoForm. We also define a global template helper called `googleMapsReady` which ensures that the Google api is correctly loaded before we attempt to render our `googleplace` input.

<script src="https://gist.github.com/quackware/a438b443ef483baf8c49.js"></script>

Don't forget to specify `type="googleplace"` for the location quickfield, or else the google places api will not be loaded!

## Client Side Templates (Search)

The second part of our client side template is another simple autoform that will perform the location search using another `googleplace` input along with a radius field.

<script src="https://gist.github.com/quackware/3946d1a6540e84b4669f.js"></script>

Notice these fields match the `Schemas.Search` SimpleSchema we defined earlier in the tutorial, and the `searchSchema` template helper should return that SimpleSchema object (similar to how the `jobSchema` template helper did above). This AutoForm template does not use `meteormethod` and instead uses `normal` which means we will be treating it as a normal form submit. Why we do that will be explained when we move on to the JavaScript portion of the client side templates.

## Client Side Templates (Display)

The third and fourth part of our client side templates is just to display the search results.

<script src="https://gist.github.com/quackware/8e62f53ebcb045def220.js"></script>

Notice for the first row we iterate over a `jobs` template helper and display the title of the job and the address where the job is located. In the second row we just add a div with id `searchMapContainer`. This is where we will be rendering our embedded Google Map. Adding all the html together produces the following template:

<script src="https://gist.github.com/quackware/fc073273309b64abcfc3.js"></script>

We also need to add a small css file to ensure our `searchMapContainer` is correctly sized

<script src="https://gist.github.com/quackware/0f48399bb59294f9e966.js"></script>

That's it for the html/css code. We can now move on to the JavaScript portion of the client side code!

## Client Side JavaScript (Rendering our Google Map)

The first part of our `index.js` file will focus on rendering the Google Map in our website so that we can later add markers to it based on the job location. We can accomplish this with the following code:

<script src="https://gist.github.com/quackware/9e0f664259dd7da3e957.js"></script>

In our `onRendered` function we create a simple `autorun` function that waits until the `GoogleMaps` api has been loaded before calling our `createMap` function. The `createMap` function just creates a `latlng` object using some default coordinates and renders a map to our `searchMapContainer` div we created in our template html code. Once this code has been added to your `index.js` you should see a page similar to below:

![Google Maps embedded](http://i.imgur.com/JOC8bWu.png)

## Client side JavaScript (Search Code)

Now that our Google Map is rendered on our website we can add the code that will subscribe to our `jobSearch` pubication and render markers on our map and populate our `jobs` template helper.

<script src="https://gist.github.com/quackware/4e2dbb0d16de90fbb489.js"></script>

Starting from our `onRendered` function we see that we have added two new `autorun` functions. The first one waits on the reactive-var `SearchData` and subscribes to our `jobSearch` publisher we created before. The second one waits on the reactive `Jobs.find({})` which when populated with data (which will occur when our `jobSearch` subscription completes) will call our `addMarkersToMap` function to render markers in the location where the job was specified.

Our `addMarkersToMap` function simply removes the existing markers that were added and iterates through the passed in jobs, added a new marker for each job based on the job's lat and lon values. We also added a `click` listener for when you click on one of the markers. You could do something like open a popup with job information or highlight the job in a list, but for now we simply output the job to the console.

Additionally we add our `searchJob` AutoForm hook that sets the `SearchData` reactive-var with data from our search form. This way when someone performs a search the `SearchData` variable is changed which triggers a new search subscription, returning our search data. That subscription then triggers the new `Jobs.find({})` which populates both the template helper and our map markers.

## Conclusion

That's it for the tutorial. We accomplished a lot in this long tutorial. The end goal is that we were able to reactively update an embeded Google Map widget based on a locaiton search we performed. If you have any questions or suggestions make sure to leave a comment! Again the full source code for this tutorial is available on [My GitHub Page](https://github.com/quackware/meteor-location-search).