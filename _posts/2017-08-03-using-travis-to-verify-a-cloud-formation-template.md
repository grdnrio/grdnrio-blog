---
layout: post
title:  "Using Travis to verify a Cloud Formation Template"
description: "Demonstrates cloud formation template validation using the Travis CI service in GitHub."
date:   2017-08-03 10:14:57 +0100
tags: [travis, cloud, cloud formation, aws]
comments: true
share: true
---
![Rusty machine](http://images.grdnr.io/2017/1_aB1oh7g5LayPo2xaoWnikA.jpg)

Deploying infrastructure in the cloud is fun. Deploying into AWS is even more fun. Clicking buttons is not fun. You should use a Cloud Formation template.


> AWS CloudFormation gives developers and systems administrators an easy way to create and manage a collection of related AWS resources, provisioning and updating them in an orderly and predictable fashion.


Maintaining a Cloud Formation template (hopefully you wrote it in yaml) brings the same challenges as maintaining any code base. How can you verify the quality of code, especially in a shared environment.

I like to use [Travis](https://travis-ci.org) to kick off simple verification tests on any pull request against my CFT.

> Travis CI is a hosted, distributed [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) service used to build and test software projects hosted at [GitHub](https://en.wikipedia.org/wiki/GitHub)

It’s a continuous integration tool that can automate the execution of verification tests such as syntax, linting and unit tests, as well as executing deploying your application to a service such as Hroku- although this isn’t relevant in the CFT example.

## How does it work?
After setting up your account you need to enable Travis as an extension for your CFT repo. This is handled through the Settings and then Services panels.

![Travis service](http://images.grdnr.io/2017/travis-hooks.png)

Once enabled you need to add a Travis file to your repo. This file controls what Travis does when a certain trigger event occurs, such as a pushed branch or a new pull request.

The following example is really simple. I’ll explain it in a minute.

{% gist cb0bb84b3e1e1812516d15a639b75537 %}

Numbers = lines

1. I’m using Ruby gems so I need Travis to configure the build node accordingly.

6.  I need to configure the AWS CLI on the build node which requires passing through credentials. This is where I set them. Travis can create a secure string for you.

12. Onto the good stuff. Here I’m specifying the steps that Travis needs to take to prepare the build node for executing my required tests. In this case I’m installing the AWS CLI and a YAML linting Ruby gem.

20. This is where we specify the actual commands to run in order to tests the codebase. Here I’m calling the aws cloudformation verify-template command against a path to my template, and the same with the ruby gem.

24. Some nice output :)

29. Notifications are important, especially when tests fail. This is notifying my team’s Slack channel at Chef. I like to turn email off as the GitHub UI + Slack are enough notifications for me.

## What’s next?

Hopefully the above gives you some idea of basic verification tests that Travis can execute. Arguably they are only saving a little bit of time as an attempted deployment with a malformed template will fail anyway. Having said that, I have a nice starting point from which to extend, and getting testing into an IT team’s culture is important.

The next step would be to add the template deployment. At least then you can check AMIs exist and a deployment creates. You may wish to follow this with a set of Selenium tests, should your stack deploy an application. Either way, I hope you find the above example useful in your own adventures with CI!