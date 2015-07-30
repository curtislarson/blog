---
layout: post
title: Meteor - Send Template Emails from the Server with Mailgun
description: Tutorial on how to send template emails populated from a Collection from the server using Mailgun.
keywords: meteor, ssr, meteor template email, meteor email, meteor server template email, mailgun, meteor email mailgun, meteor collection email, meteor template email collection
author: curtis_larson
comments: true
---

## Introduction

In this tutorial I will show you how I have been sending template emails from the server with Mailgun. This is very useful if you want to send transactional or newsletter emails using a pre existing template, but you also want to dynamically populate the email with data from one of your Collections. The full code for this tutorial is available [on my GitHub page](https://github.com/quackware/meteor-template-email). If you have any questions please leave them in the comments!

## Dependencies

This tutorial requires the following dependencies (which will be auto downloaded if you clone the tutorial directly):

    iron:router
    meteorhacks:npm
    meteorhacks:ssr

Additionally you need to specify the following `meteorhacks:npm` dependency in a `package.json` file in your root directory:

    {
        "mailgun": "0.5.0"
    }

## The Email Template

For this tutorial I chose a basic email template (modified slightly for Meteor specific stuff) from the mailgun website. It is **IMPORTANT** that you put the template file in the `private/` directory of your project so that it can be accessed by `Assets.getText` on the server. I created the following file and placed it at `private/email-template.html`. Note the placeholders `mainTitle`, `tasks`, `task.title`, `task.url`, and `unsubscribe`. You will see how we will replace those placeholders with data from our collection later.

<script src="https://gist.github.com/quackware/341cd73d196236f78b31.js"></script>

## Startup Code

Now that we have our email template, we need to add some code to our server that is run on startup. I created a file called `startup.js` that will setup our template rendering system and setup our mailgun settings. The code can be seen below:

<script src="https://gist.github.com/quackware/4f30b6007e89a45766b9.js"></script>

Notice the creation of a `templates` array, which we push a single name and path to. The name is used when we want to reference a specific template when we send an email and the path references the relative location of the template html file to the `private/` directory. This template array is passed to a `EmailGenerator` object which we will implement in the next section. Another important thing to note is the various `Meteor.settings` variables I use in setting up my stmp `MAIL_URL`. These variables are read from a `settings.json` file that is loaded when you start meteor like so:

    meteor --settings settings.json

An example settings.json file for this tutorial would look something like this:

<script src="https://gist.github.com/quackware/b70c9cf3c43fa0e01c13.js"></script>

## Mailgun

The mailgun code is composed of a single `Meteor.method` that takes in basic email information and forwards it to the mailgun api we imported using `meteorhacks:npm`.

<script src="https://gist.github.com/quackware/41cd6ba173d14a66557a.js"></script>

Note that we use `sendRaw` here to take advantage of html emails, which requires us to format the rest of the email body manually.

## EmailGenerator

The `EmailGenerator` object contains two simple methods, one we saw above that compiles each template html file using `meteorhacks:ssr`, and another that generates the html from the compiled template. You can see the code for the `EmailGenerator` below:

<script src="https://gist.github.com/quackware/820b7cc9f7f5f7ee24ec.js"></script>

A few important things to note is the `templateName` argument of `generateHtml` which is used to referene the template we passed in with `addTemplates`, and also the `data` parameter of `generateHtml`. The `data` parameter is what will populate the various handlebar templates in our `email-templates.html` file. This data will come from our Meteor collection we set up in the next section.

## Server Code

The server code is composed of two very basic `Meteor.methods`: `addTask` and `sendEmail`. Both of these methods will be called from the client code that we implement in the next section.

<script src="https://gist.github.com/quackware/1f06550d64a7770a8fad.js"></script>

`addTask` does exactly what it's name implies, adds a task composed of a title and url to a Mongo.collection we created with

    Tasks = new Mongo.Collection("tasks");

specified in a seperate javascript file (You can add that line anywhere in the server code). `sendEmail` is slightly more complicated, but basically acts as the glue between all our previously implemented code. It pulls all the tasks from the database, creates that data object that we then pass to `EmailGenerator.generateHtml`, and calls the `sendMailGunEmail` method with the generated email html which forwards the email to our mailgun api.

## Client Code

Finally we need to create some basic client code where the user can enter a title and a url for a "task". This task data will be sent to the server where we will insert it into the `Tasks` collection. We can then pull data from this `Tasks` collection and use it to populate our email template that we will create.

In addition to adding tasks, we will let the user be able to click a button to send an email. This code will just perform a basic `Meteor.call` which will call into our template generating and email sending code on the server. You can see the two files, `index.html` and `index.js` below:

<script src="https://gist.github.com/quackware/831b9cb8df0e9c849adb.js"></script>

<script src="https://gist.github.com/quackware/57643c6fe549eab61695.js"></script>

## Conclusion

Hopefully this helps our anyone that was interested in how to send template emails from the server with mailgun. Again the full code for this tutorial is available [on my GitHub page](https://github.com/quackware/meteor-template-email). If you have any questions please leave them in the comments!