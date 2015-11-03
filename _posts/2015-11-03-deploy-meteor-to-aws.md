---
layout: post
title: How to Deploy a Meteor project to AWS EC2
description: Guide on how to deploy a meteor project to AWS Amazon Web Services EC2
keywords: aws, amazon web services, meteor, meteor deploy, meteor deploy aws, aws meteor guide, meteor aws guide, meteor deploy aws guide, meteor deploy amazon, amazon meteor, meteor ec2, meteor deploy ec2, amazon ec2, aws ec2, ec2 deploy meteor
author: curtis_larson
comments: true
---

## Introduction

This is a step by step guide on how to deploy a Meteor project to Amazon Web Services (AWS). This guide assumes you have little to no technical knowledge of AWS or Meteor and you start out with a zipped file containing your Meteor project. It also assumes you have knowledge of basic unix commands like cd, ls, and mkdir. I am also assuming you are using a mac / linux based computer. Let's get started!

## The Tools

The first tool you need is Node.js which is available [here](https://nodejs.org/en/). After you have node installed you can bring up your terminal and type `npm` to make sure you have the Node package manager installed correctly (See below image).

![npm](http://imgur.com/HAQGTua.png)

Once you verify that npm is installed you need to install [Meteor Up](https://github.com/arunoda/meteor-up) through npm which will help us deploy the project to an ec2 server. You can install it by typing the following command in your terminal.

    npm install -g mup

![Install mup npm](http://imgur.com/pylYMVS.png)

You also need to install meteor

    npm install -g meteor

You now need to navigate to your project directory via the terminal. If you are familiar with the `ls` and `cd` unix commands it should be pretty straightforward.

    cd /path/to/project/directory

If you are unfamiliar with unix commands you can navigate to System Preferences -> Keyboard -> Services (Left Hand Side) -> Scroll down and check the `New Terminal at Folder` checkbox.

![New Terminal At Folder](http://imgur.com/1EsrGt2.png)

Now you can right click your project folder in Finder -> Services -> New Terminal at Folder which will open a terminal in your project directory.

![New Terminal At Folder](http://imgur.com/SbQ6z6w.png)

Now that you are in your deployment directory you must create a new file named `mup.json` and copy and paste the below code and save the file (You can do this either through the Terminal or Finder).

<script src="https://gist.github.com/quackware/4aad8874a59ec2624e4c.js"></script>

We will be changing some of these values later on in the tutorial once we have setup our server.

## AWS Management Console

Login to your [AWS Management Console](https://aws.amazon.com/console/) and you should see a screen similar to below (If you don't have an AWS account, they are very straightforward to create and is not covered by this tutorial)

![AWS Console](http://imgur.com/RThAxXC.png)

Click on the `EC2` option in the top left and select the blue `Launch Instance` option. On the next screen you should see a selection of different server types. You will want to pick the `Ubuntu Server SSD Volume Type` since it works best with `mup`.

![Ubunutu AMI](http://imgur.com/u6kwg7w.png)

You will now be greeted with a selection of different instance types. For my simple project I will be using a free tier option, but you should do your research or [ask a meteor expert](mailto:curtis@curtismlarson.com?subject=I need help setting up my Meteor project!) if you are unsure on what tier option to use. Go ahead and select `Next: Configure Instance Details` to continue.

## Setting up Server Options

The next steps will be based on what type of app you have. Most people will not need to change anything on the `Configure Instance Details` page, so go ahead and select `Next: Add Storage`.

If you think your app requires additional storage (most will not as long as you chose the correct server in the above app) you can add volumes in this screen. If anyone is curious on how to add additional volumes for their Meteor app just leave a comment and I will update the guide

On Step 5: Tag Instance, just give your server a useful name that reflects your meteor project and proceed to the next step

## Configure Security Groups

Setting up correct security groups is important for any app you host on AWS. You want to allow ssh traffic for mup and if you want to ssh into your server in the future, and http/https traffic for people visiting your website. You can also specify ssh rules for only your IP address as an additional security precaution. Using the below configuration should work for most people

![Configure Security Group](http://imgur.com/2M7BaiE.png)

Now that you have added your security groups you can go ahead and click `Review and Launch`.

## Keypair & Launching Your Instance

After reviewing your instance information you can go ahead and click `Launch`. This will launch a popup asking you to use an existing or create a new keypair. We are going to create a new one and download it. It is very important that you keep the keypair in a secure location, this file will be used by `mup` and anyone else that wants to ssh into your server (As long as they are added to the ssh rules for your instance). Once the file is downloaded click `Launch Instance` again. Go ahead and click `View Instance` and you will be directed to your instance dashboard.

![Creating keypair](http://imgur.com/fEXcJ50.png)

## Getting instance information

Now that we have successfully launched our instance we need to get some information from the instance dashboard so that mup can deploy the meteor project. Once the instance is running select it from the list and record the `Public IP` value.

![Public IP](http://imgur.com/0S5c3hl.png)

Now navigate to where you saved your .pem file via the terminal (either through unix commands or the New Terminal at Folder method). Type the following command into the terminal:

    chmod 400 name-of-pem.pem

![chmod](http://imgur.com/2WVjf2g.png)

This will allow ssh and mup access to your pem file. You can now test to see if you can ssh into your server with the following command:

    ssh -i /path/to/pem.pem ubuntu@public-ip-address

![SSH amazon](http://imgur.com/TZLBgqW.png)

If you have successfully connected to your server, Congratulations! Now all you need to do is edit your `mup.json` file.

## Editing the mup.json file

Once your `mup.json` file is open, you need to change the following values:

    "host": "hostname" -> "host": "PUBLIC_IP_ADDRESS"
    "username":"root" -> "username":"ubuntu"
    "password":"password" -> //"password":"password"
    //"pem":"~/.ssh/id_rsa" -> "pem":"path/to/your/pemfile.pem"
    "app": "/path/to/the/app" -> "app": "[the local path of your app]",
    "appName":"meteor" -> "appName":"[your-app-name]"
    "ROOT_URL":"http://myapp.com" -> "ROOT_URL":"[whatever your url is]"

Save and exit your text editor.

## Deploying with Mup

Navigate to your project directory in a terminal and run the following commands:

    mup setup

If all goes well you should see the following output. If something bad happens double check all your values in your mup.json file

![mup setup](http://imgur.com/GNONEGV.png)

Now that mup has installed all the necessary requirements for Meteor on your server. You can type the following command to upload your project to the server.

    mup deploy

![mup deploy](http://imgur.com/4uGXf6s.png)

That's it! You can now check out your app either directly through your public ip address or through the ROOT_URL you set (as long as your DNS records are set up correctly). If you have any questions or requests for additions to this guide, just leave them in the comments!

