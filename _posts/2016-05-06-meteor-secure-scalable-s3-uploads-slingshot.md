---
layout: post
title: Meteor - Secure, Scalable Uploading to S3 with Slingshot (Video Presentation)
description: A video presentation on how to build secure, scalable uploading to Amazon S3 with Slingshot and Meteor
keywords: meteor, meteor slingshot, meteor s3, meteor s3 tutorial, meteor upload s3, meteor amazon s3, meteor s3 upload, meteor slingshot tutorial, meteor upload slingshot
author: curtis_larson
comments: true
---

## Introduction

Often times when you first build a file upload tool in Meteor you look towards using a package like [CollectionFS GridFS](https://atmospherejs.com/cfs/gridfs) to store files directly in MongoDB. However as more and more files are uploaded it can reduce your database speed to a crawl. A much better solution is a completely client side file upload implementation powered by [Slingshot](https://github.com/CulturalMe/meteor-slingshot) and [Amazon S3](https://aws.amazon.com/s3/).

I recently gave a presentation at the [Meteor NYC Meetup](http://www.meetup.com/Meteor-NY) with my co-presenter [Adrian Lanning](https://github.com/alanning) where we gave a short demo and talked about the functionality of building a secure, scalable uploading tool with Slingshot and S3. Luckily the presentation was recorded so you can watch it below! As always the full source code of the presentation can be found on [My Github](https://github.com/quackware/meteor-slingshot-example). Enjoy!

## Video Presentation

<iframe width="640" height="360" src="https://www.youtube.com/embed/9VAldOPkqqs" frameborder="0" allowfullscreen></iframe>

## Conclusion

Those with an existing GridFS implementation or that just want to add a great file upload tool to their Meteor website can contact me below to start a discussion and receive a quote.