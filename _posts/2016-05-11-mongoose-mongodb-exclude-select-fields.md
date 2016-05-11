---
layout: post
title: Node - Exclude fields from select with Mongoose/MongoDB
description: How to exclude certain fields from being selected when using Mongoose with MongoDB
keywords: node, node mongoose, node mongodb, mongoose exclude, mongoose exclude field, mongodb exclude field, mongoose mongodb exclude, mongoose exclude password
author: curtis_larson
comments: true
---

## Excluding a Field

Say you have a mongoose schema for a user as follows:

    var userSchema = mongoose.Schema({
      email: {
        type: String,
        required: true,
        unique: true,
      },
      password: {
        type: String,
        required: true,
      },
    });

    var User = mongoose.model("User", userSchema);


But you don't want to send the `password` field to the client when you perform a `User.find`. Rather than directly deleting the `password` field from the user object before you send it to the client, mongoose provides some easy to use functionality to exclude your `password` field. There are two ways you can accomplish this.

### Individually

    User.findOne({_id: userId}).select("-password")


### In the Schema

    var userSchema = mongoose.Schema({
      email: {
        type: String,
        required: true,
        unique: true,
      },
      password: {
        type: String,
        required: true,
        select: false,
      },
    });


Notice the new `select: false` field in the password field of the User schema. Now whenever you perform a find with either method, the password field will not be included in the response.

## Including the Excluded Field

But what if you need to check the password with a value (Such as in the login method)? You can explictly allow the password field to be returned in your find call as follows:

    User.findOne({_id: userId}).select("+password")

## Conclusion

That's it for this tutorial. This is very common functionality in any Node/MongoDB/Mongoose related application, and avoiding publishings fields that can compromise your application is always a good idea!