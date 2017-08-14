---
layout: post
title:  "Scanning Docker containers with InSpec"
description: "Using InSpec to scan and verify the state of a Docker container"
date:   2017-08-14 13:07:11 +0100
tags: [inspec, security, docker]
comments: true
share: true
published: false
---

Docker makes running containers incredibly simple, a big reason for its popularity. I can quickly and easily run an Nginx container on my workstation, whether Mac, Windows or Linux based.

docker container run --publish 80:80 --detach --name nginx nginx

And as if my magic...

![Nginx local host](http://images.grdnr.io/2017/docker-nginx.gif)

