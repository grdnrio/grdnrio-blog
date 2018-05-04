---
layout: post
title: InSpec and Chef - compliance as code
description: InSpec and Chef examples from the Chef Community Summit 2016 talk
date: 2016-10-22
tags: [chef, inspec, community]
comments: true
share: true
---
![Community Summit presentation](https://images.grdnr.io/2017/inspec-community-talk.jpg)

InSpec is an opensource language that can be used to assess the state of systems. It can form integration tests, but more importantly, with additional meta information, can create so called compliance profiles. These represent either business requirements or industry standards such as ISO 27001 and CIS.

On the 12th October I presented with [Christoph Hartmann](https://de.linkedin.com/in/chrihartmann) at the London Chef Summit on the subject of InSpec. We spoke about the changes in InSpec 1.0 and gave a demo showing how you can use InSpec in your cookbooks for integration tests, and to produce compliance profiles that can be applied at all stages of the development process.

Here are the resources from our talk!

<iframe src="https://docs.google.com/presentation/d/1ygMFoY2vgKIPqOUQRMRhtL7ZxO2m3485yaXsSR9pRcc/embed?start=false&loop=false&delayms=3000" frameborder="0" width="640" height="360" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

## Resources

**Simple web cookbook** - [https://github.com/grdnrio/inspec-summit](https://github.com/grdnrio/inspec-summit)
Here you will see a .kitchen.yml file that contains the runlist for os and ssh hardening taken from the metadat.rb dependencies that we used in our demo. You'll also find the website style attributes in the default location.

**OS and SSH baseline InSpec profiles:**
Use the following profiles to assess state.
	
  * OS profile - [https://github.com/dev-sec/tests-os-hardening](https://github.com/dev-sec/tests-os-hardening)
	
  * SSH profile - [https://github.com/dev-sec/tests-ssh-hardening](https://github.com/dev-sec/tests-ssh-hardening)

**Example corporate profile**
This repo shows how you can build a single profile to address all of your compliance scanning needs with InSpec. This example, used in the presentation, shows how you can include upstream profiles, skip controls, and also include your own InSpec tests. It also demonstrates platform awareness, showing how a prpfile can be platform agnostic and therefore applied holistically. 

[https://github.com/chris-rock/acme-inspec-profile](https://github.com/chris-rock/acme-inspec-profile)

**OS and SSH hardening cookbooks:**
	
  * OS hardening - [https://github.com/dev-sec/chef-os-hardening](https://github.com/dev-sec/chef-os-hardening)
	
  * SSH hardening - [https://github.com/dev-sec/chef-ssh-hardening](https://github.com/dev-sec/chef-ssh-hardening)

Finally the best place to check for everything InSpec is the brand new website - [inspec.io](http://inspec.io)
