---
layout: post
title:  "Automating Windows Local Security Policy"
description: "Using Chef to automate the application of local security policy to a Windows Server"
date:   2017-08-03 10:44:57 +0100
tags: [chef, windows, security]
comments: true
share: true
---
![Chef Windows](http://images.grdnr.io/2017/chef-windows.png)

Enforcing security policy is tough, especially in a Windows environment where you are NOT using Group Policy. Think about usage patterns for Windows server on cloud. GPO certainly doesn’t always apply. In fact, Microsoft actually recommend GPO for Desktop / Workstation use and other solutions, such as Chef and DSC for servers.

So then, how should one manage Local security policy on servers, at scale?
Fortunately there’s a decent automation tool you may have heard of… Chef!
I wrote a cookbook for managing local security policy. It offers the following features:

* Idempotent execution of policy via secedit.exe
* Exporting of security databases
* Import and configure options
* Custom security databases
* Security policy generation via template

Take a look at this example custom resource wrapper for secedit.exe.


{% gist 7d90f141f7584a9c25ebeca1f28059e9 %}


## Using this cookbook
This is a helper cookbook so you need to make sure you add it to your metadata file in another cookbook. A good example is the Dev-Sec project’s Windows Hardening cookbook.

This cookbook establishes a baseline of hardening on Windows Servers (2012 through to Nano).

Check out the metadata file, it includes my security policy cookbook.


{% gist 27f8f0f3f8389d60bec8d77558813063 %}


By including this cookbook, you then have access to the resources. You may want to use them as follows in this example in any normal recipe you may write.


{% gist b5dd3acd1013ad9a95c6eaa1ae4ec966 %}

## What’s next?
This is an open source project, so I’d love your contributions. I’m working on a custom resource for managing Audit Policy in the next release. If you’d like to get involved… or report a bug… check out the GitHub project.

[View the complete cookbook](https://github.com/grdnrio/windows-security-policy)