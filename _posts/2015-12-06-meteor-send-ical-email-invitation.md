---
layout: post
title: Meteor - How to Create and Send an iCal Invitation Email
description: How to create and send an iCal Invitation Email using Meteor.
keywords: meteor, meteor email, email, meteor ical, meteor attach ical, meteor ical attachment, meteor send ical, meteor ical email, meteor attach ical email, meteor email ical, meteor calendar, meteor attach invite, meteor invitation email, meteor email invitation
author: curtis_larson
comments: true
---

## Introduction

In this tutorial I will show you how to send an iCal invitation email using basic Meteor packages and an iCal generator smart package. iCal files are useful because they allow the email recipient to instantly add the invitation or event to their calendar program directly form their email client. If you send emails with any sort of event information you should be sending an iCal file! The full source code for this tutorial is available on my [GitHub page](https://github.com/quackware/meteor-send-invitation).

## Setup

There are a few packages you need to get started

    meteor add email
    meteor add quackware:ical-generator

The email package is pretty straightforward, it's the basic email package provided by Meteor. The ical-generator is the npm package [ical-generator](https://www.npmjs.com/package/ical-generator) that has been [wrapped as a meteor smart package](http://www.curtismlarson.com/blog/2015/11/20/meteor-wrap-publish-npm-package/). You can check out the source code of the smart package [here](https://github.com/quackware/meteor-ical-generator).

You also need to sign up for some sort of 3rd party email service (sendgrid, mandrill, etc) so that you can set your `MAIL_URL` variable in the server, allowing you to send emails. For this tutorial I used a mandrill account, but they are all very similar to use.

## Client Side

The client side code for this tutorial is very basic. We just need to have a few fields in a form that we can submit to the server. You can see the `send-invitation.html` and `send-invitation.js` files below:

<script src="https://gist.github.com/quackware/bdb509817ef76af80efa.js"></script>

<script src="https://gist.github.com/quackware/de8d369ff3f979989573.js"></script>

These fields are just a sample of what you can include in an ical file. You can check out the documentation for ical-generator [here](https://www.npmjs.com/package/ical-generator) which should give you a good idea of additional fields you might want to add.

## Server Side

The first part of the server side code is setting up the `MAIL_URL` variable. I am setting it based on the user and password variables I have stored in my `settings.json` file:

<script src="https://gist.github.com/quackware/30ce2c9b9a96440098a3.js"></script>

You can read a more in depth discussion of setting up the `MAIL_URL` variable and the `settings.json` file in my [send template emails from the server tutorial](http://www.curtismlarson.com/blog/2015/07/30/meteor-send-template-emails-from-server-mailgun/). That tutorial uses mailgun, but mandrill works the exact same way.

We also need to implement the `sendInvitation` Meteor method we referenced in the client side code. This code uses our `quackware:ical-generator` package we already added.

<script src="https://gist.github.com/quackware/34c8c0dab4d4de680ecd.js"></script>

As you can see we create an empty ical object and then add an event to the calendar object populated with the data from the client side. We can then use `cal.toString()` to attach the ical file as an attachment using the built in `Email.send` function provided by Meteor. That's it! Once you test it out and send an email you should see an email similar to the one below:

![Example Ical Email](http://i.imgur.com/s4uACyM.png)

## Conclusion

Thats it! Meteor makes it very easy to both send emails and include additional packages like the ical-generator to add additional functionality to our application. Again the full source code for this tutorial is available on my [GitHub page](https://github.com/quackware/meteor-send-invitation). If you have any improvements or suggestions make sure to leave a comment!
