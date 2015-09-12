---
layout: post
title: Meteor - Dynamically Render a PDF with iron:router
description: How to dynamically render a pdf from the server to the client using iron:router
keywords: meteor, pdf, meteor pdf, meteor render pdf, meteor iron:router, meteor iron router, meteor iron router pdf, iron router pdf, iron:router render pdf, meteor render pdf server iron:router, dynamically render pdf, iron:router dynamically render pdf, render pdf dynamically
author: curtis_larson
comments: true
---

## Introduction

A cool trick I found out recently is how to dynamically render a pdf from the server to the client using iron:router. The idea is you have a pdf object stored in a database or external file store and you want to transfer that pdf to the client without directly accessing the pdf file. We are going to accomplish this with a server side iron:router route. You can access the full code for this tutorial on my GitHub page [here](https://github.com/quackware/meteor-render-pdf).

## The Route File

The first method of rendering the pdf from the server involves the wonderful [CollectionFS](https://github.com/CollectionFS/Meteor-CollectionFS) package which can be used to store files in a variety of different data stores including mongoDB, dropbox, and Amazon S3. We can accomplish this with the below code:

<script src="https://gist.github.com/quackware/c13931a2e2c9b4216b88.js"></script>

Some important thigns to note include the creation of a readable object through `file.createReadStream("tmp")` which we then read into the `buffer` object. Once we have completely read from the `FS.File` object we can write the response to the client through the node `this.response.write(buffer);` statement. Also make sure to include the `where: "server"` in the route to ensure that it only executes as a server side route.

An additional way to render a pdf from the server to the client is to read the pdf from the file system, read it into a buffer using the node.js `fs` package, and feed it to the client. Make sure you install the `meteorhacks:npm` package so you can include the `fs` node.js package. You can accomplish all this with the below code:

<script src="https://gist.github.com/quackware/52d139c978bb05535cff.js"></script>

Pretty straight forward and similar to the above gist.

## Conclusion

That's it for this tutorial, hopefully it helps out anyone who wants to dynamically render a pdf from the server without directly accessing the file. Again the full code for this tutorial is available [on my GitHub page](https://github.com/quackware/meteor-render-pdf). If you have any questions please leave them in the comments!