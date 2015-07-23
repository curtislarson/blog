---
layout: post
title: Meteor - Windows update cannot convert null to object
description: Fix windows meteor update cannot convert null to object
keywords: meteor, windows, meteor update, cannot convert null to object, meteor update windows, meteor update windows error
author: curtis_larson
comments: true
---

## Cannot Convert Null To Object

While attempting to update one of my meteor projects on my windows box, I ran into the following error:

![Cannot convert null to object](http://i.imgur.com/NvtJjoq.png "Cannot convert null to object")

Attempting to update the meteor binary through `meteor update` yielded the same error.

## How to Fix

In order to fix the error, I actually had to completely uninstall meteor and reinstall from [https://win.meteor.com/](https://win.meteor.com/). After everything was installed, `meteor update` worked fine. Hope this helps anyone encountering the same problem!