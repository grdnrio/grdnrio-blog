---
author: joe@grdnr.io
comments: true
date: 2016-10-22 19:42:06+00:00
layout: post
link: http://grdnr.io/inspec-and-chef-compliance-as-code/
slug: inspec-and-chef-compliance-as-code
title: InSpec and Chef - compliance as code
wordpress_id: 74
categories:
- Chef
- InSpec
---

InSpec is an opensource language that can be used to assess the state of systems. It can form integration tests, but more importantly, with additional meta information, can create so called compliance profiles. These represent either business requirements or industry standards such as ISO 27001 and CIS.

On the 12th October I presented with [Christoph Hartmann](https://de.linkedin.com/in/chrihartmann) at the London Chef Summit on the subject of InSpec. We spoke about the changes in InSpec 1.0 and gave a demo showing how you can use InSpec in your cookbooks for integration tests, and to produce compliance profiles that can be applied at all stages of the development process.

Here are the resources from our talk!



[embeddoc url="http://grdnr.io/wp-content/uploads/2016/10/Summit-Inspec-1.pptx" width="80%" height="80%" download="none" viewer="microsoft"]


  



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
