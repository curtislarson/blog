---
layout: post
title: How to point a domain on Google Domains to GitHub pages
author: curtis_larson
---

# Table of Contents

1. [Introduction](#introduction)
2. [Setup repository](#setup-repository)
3. [Configure Google Domains](#configure-google-domains)
4. [Conclusion](#conclusion)

## Introduction

When setting up my [website](http://www.curtismlarson.com) on Google Domains I had to read through several different guides to figure out all I needed to do to point it to a GitHub pages repository. I wrote up this guide to simplify the processes for anyone else performing this same task.

## Setup repository

Navigate to [https://github.com/new](https://github.com/new) and create a repository with the name **USERNAME.github.com** or **USERNAME.github.io** and give it a nice description.

![Create a repository](http://i.imgur.com/zQ5BxEH.png "Create a repository")

In your new repository, create a **CNAME** file in the root directory and add the following two entries (with curtismlarson replaced by your own domain):

![CName](http://i.imgur.com/LR37up5.png "CName")

These two entries tell GitHub to redirect any requests to **USERNAME.github.io** to the domain specified in the CNAME file. There are two entries to ensure that **curtismlarson.com** will also redirect to **www.curtismlarson.com**. If you prefer to have a site without the **www** prefix, you can switch the order of the domains.

Now verify that your **USERNAME.github.io** domain is now pointing to your custom domain by going into the repository settings and verifying your GitHub Pages settings:

![Github Pages](http://i.imgur.com/RsA5XUO.png "GitHub Pages")

## Configure Google Domains

Navigate to [https://domains.google.com/registrar](https://domains.google.com/registrar) and select the **DNS** option to configure your DNS records.

![Google Domains](http://i.imgur.com/oA40Qkq.png "Google Domains")

Scroll to the very bottom of the page and add 3 Custom Resource Records. You need to add two "@" type A records that point to the GitHub ips **192.30.252.153** and **192.30.252.154** and one "www" CNAME record that points to your **USERNAME.github.io** url:

![Custom Resource Records](http://i.imgur.com/xO3At1V.png "Custom Resource Records")

## Conclusion

That's it! DNS records often take more than a day to propagate so you may not see your website immediately. Once the changes have propagated your GitHub page and any project pages should be accessible from your new domain. For example [https://github.com/quackware/blog](https://github.com/quackware/blog) is hosting the blog you are reading right now and the domain works without any additional setup.