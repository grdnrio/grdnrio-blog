---
author: joe@grdnr.io
comments: true
date: 2016-10-07 18:20:56+00:00
layout: post
link: http://grdnr.io/synergy-windows-7-server-rhel-client/
slug: synergy-windows-7-server-rhel-client
title: 'Synergy: Windows 7 Server - RHEL Client'
wordpress_id: 6
---

I recently attempted to pass and failed the RHCSA exam. In order to make sure that on my second attempt I would pass I borrowed a nice meaty workstation from work and set up SL 6 on it. This workstation’s CPU has the vtx flag allowing me to create SL / Centos 6 guests to practice with.




Everything went smoothly however the use of an extra keyboard and mouse, and the space these devices took up started to become an issue. I knew there was software (I find [KVM](https://www.amazon.co.uk/s?ie=UTF8&x=0&ref_=nb_sb_ss_i_0_3&y=0&field-keywords=kvm%20switch&url=search-alias%3Daps&sprefix=kvm&_encoding=UTF8&tag=shilon-21&linkCode=ur2&camp=1634&creative=19450)s clumsy and prone to crashes) on the market that would allow a client -> server relationship to be established for the use of a single keyboard and mouse.





There’s a huge list of software available, but the only option I could find that is multi-platform is [Synergy](http://synergy-foss.org/). Don’t get me wrong, I didn’t feel forced into using Synergy! It’s well supported and Open Source so is an excellent option, and of course as it’s Open Source there are loads of third party versions with GUIs of varying quality.





[![](http://images.grdnr.io/2011/09/synergy.jpg)](http://images.grdnr.io/2011/09/synergy.jpg)





I decided to have my Windows 7 x64 box as the server in this relationship. I chose this mainly because it’s my primary computer on my desk so makes sense to have the keyboard and mouse connected to it. Having to turn on my RHEL and Win box just to use my Win box day to day would be a nightmare!





So, getting started I downloaded and installed Synergy from the link above. Installing it on Windows was very simple, it’s just an executable installer. Installing on Red Hat was slightly more challenging. I wanted use a GUI on RH for easy configuration which led to a bit of research. In the end I settled on Quick Synergy which is available from the [Google code project](http://code.google.com/p/quicksynergy/) site.





## Windows Configuration





Synergy on Windows comes with a nice simple GUI for configuration. My configuration as a server is as follows…





## RHEL Configuration





Remember, I wanted my RHEL box to be the Synergy client and unfortunately there are less guides for achieving this. The simple steps to manually run Synergy are here in [Red Hat Magazine](http://magazine.redhat.com/2007/10/18/sharing-a-keyboard-and-mouse-with-synergy/). Scroll down to find the client configuration options.





To make Synergy run at boot is quite tricky. I ended up using the following guide. It’s important to ensure you are using your X-Windows manager otherwise you’re pretty much on your own. To make Synergy run at boot, you need to run it with X. Here’s the guide from the Synergy [auto start wiki](http://synergy-foss.org/pm/projects/synergy/wiki/Autostart).





* * *





If you have any problems getting your Synergy to work leave a comment and I’ll happily share my config files.
