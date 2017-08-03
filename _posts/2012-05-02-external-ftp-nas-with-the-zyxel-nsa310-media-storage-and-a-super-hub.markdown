---
layout: post
title: External FTP NAS with the ZyXEL NSA310 Media Storage and a Super Hub
date: 2012-05-02
tags: [linux]
comments: true
share: true
---

I have a lot of films / music / software / games stored in various places and on various devices in my home. Keeping all this data organised is an absolute nightmare, and with a recent string of disk failures, I’ve also lost quite a lot of it. Following the recent death of an external [USB HD](http://www.amazon.co.uk/s?url=search-alias%3Daps&field-keywords=external+usb+hard+drive&x=0&y=0&_encoding=UTF8&tag=shilon-21&linkCode=ur2&camp=1634&creative=6738) I was in the market for a newer model. Looking around it made sense to invest a little more in a NAS to centralise storage on my home network and offer the redundancy of RAID 1 (across two disks, the 4 bay NASs are a tad outside my budget!).




The device I selected is the [ZyXEL NSA310](http://www.amazon.co.uk/gp/product/B005LDM09U/ref=as_li_tf_tl?ie=UTF8&tag=joeg-21&linkCode=as2&camp=1634&creative=6738&creativeASIN=B005LDM09U), mainly because it was a good price, but also because it has an SMB, FTP and NFS server built in, as well as a decently reviewed web admin panel. Setting it up on my network was incredibly simple. In the box is a setup utility disk that when installed, offers a ‘Quick Start’ wizard to take you through the basic steps to configure the NAS. This utility is Windows only, however it isn’t really required after the intial config.





The NSA310 has a gigabit port in the back so my initial heavy data transfer went quite quickly at around 30 (peaking at 40) Mbps. I then setup the FTP server with a few users to allow external access. This was all completed through the web admin panel (when logged in as the admin user)…





[![](http://images.grdnr.io/2011/11/ftp-admin.png)](http://images.grdnr.io/2011/11/ftp-admin.png)





The only complication was to allow external network access to the FTP server was setting up port forwarding on my rubbish Virgin Media Super Hub. Setting up forwarding rules was actually very simple, but that doesn’t stop the Super Hub from being rubbish.







  1. Log into your Hub by IP (probably 192.168.0.1) and enter you login details.  


  2. Click on the ‘Advanced Settings’ link at the bottom of the page.  


  3. Under the advanced title in the sidebar, click on the Port Forwarding option. This will load the control panel for forwarding.  


  4. Now you just need to fill out the fields to create a rule…   

![](http://images.grdnr.io/2011/11/forwarding-table.png)



  5. **Name:** You can call your rule anything you like. I called it NAS FTP.  



  6. **Start Port -> End Port:** Enter 20 for the start port and 21 for the end port. These are the two ports FTP most commonly uses.  


  7. **Protocol:** The protocol is TCP.  


  8. **Local IP Address:** This is the IP of NAS on your network, in my case 192.168.0.100.  


  9. Click Add, and then log out and you’re finished.





![](http://images.grdnr.io/2011/11/forwarding-table-complete.png)





Now you can FTP with the FTP user or admin user you created at your ISP IP and the FTP traffic will automatically be forwarded to your NAS by your router.





**UPDATE:** I have written a follow up guide for connecting to your NAS using the FileZilla FTP cleint, it is available at, “[Remotely connecting to your NAS](http://joegardiner.co.uk/remotely-connecting-to-your-nas-with-ftp/)“.
