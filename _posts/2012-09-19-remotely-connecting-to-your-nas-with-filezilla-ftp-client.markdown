---
author: joe@grdnr.io
comments: true
date: 2016-10-07 18:20:59+00:00
layout: post
link: http://grdnr.io/remotely-connecting-to-your-nas-with-filezilla-ftp-client/
slug: remotely-connecting-to-your-nas-with-filezilla-ftp-client
title: Remotely connecting to your NAS with FileZilla FTP client
wordpress_id: 14
---

I have had a few questions posted on a previous post, “[Setting up port forwarding on a Virgin Media SuperHub with a Zyxel NSA310 NAS](http://joegardiner.co.uk/external-ftp-nas-with-the-zyxel-nsa310-media-storage-and-a-super-hub/)” that ask about how to connect to the NAS remotely after setting up port forwarding. This post is a quick guide for using the [FileZilla](http://filezilla-project.org/) FTP CLIENT to do this.




FileZilla is cross-platform compatible so you can follow this guide using the FileZilla client on Linux, Mac OSX, or Windows.







  * Head to the FileZilla [Client download page](http://filezilla-project.org/download.php?type=client) and select the option most suitable for your computer. Take note if you are a Windows user, it is easier to go with the Setup.exe version which you can [download directly here](http://sourceforge.net/projects/filezilla/files/FileZilla_Client/3.5.3/FileZilla_3.5.3_win32-setup.exe/download). Also, OSX users make sure you choose PowerPC or Intel depending on your system.



  * When the client is downloaded install it on your system. I’m not going to tell you how to do that… If you do get stuck there is a [documentation section](http://wiki.filezilla-project.org/Documentation) on the FileZilla website.



  * Now you need to find out what your IP address is as assigned to you by your ISP. You can do this by going to Google and searching “My IP”. At the top of the results your IP address will be shown! Take a note of this, I would copy and paste it into a text file for later use.



  * Launch the FileZilla client you just installed. For now all you need to worry about is the bar at the top of the window that has the **Host**, Username, and **Password** fields. We are going to fill these out now to connect to your NAS.   

[![](http://images.grdnr.io/2012/04/Screen-Shot-2012-04-07-at-10.51.16.png)](http://images.grdnr.io/2012/04/Screen-Shot-2012-04-07-at-10.51.16.png)

1. In the **Host** field enter the IP address you just got from Google.



  * In the **Username** field enter either the admin username, or the user you created in the web interface or setup utility for your NAS.



  * In the **Password** field enter the password you chose for the admin account or the password you chose for the user you created.






**N.B.** If you do not know how to create a user or have not done so refer to your NAS documentation, or access the web interface locally (at home) for your NAS and follow the user wizard in the Administration section.







  * You can leave the port field blank, unless you have chosen a different FTP port when you set up port forwarding instead of the default which I suggested. If you have done this then you probably don’t need this guide!



  * When all the details have been added you can click Quick Connect to attempt a connection to your NAS. If successful you will see the right hand side of the FileZilla window populate with the directories on your NAS. You can now browse around them as if it were any other filesystem on your computer. To download files you can drag them from the right hand panel to the left hand one which is your local filesystem of the computer you are using.






### Troubleshooting





If you are unable to connect there may be one of the following problems.







  * **You may have restarted your router since following this guide and your ISP may have assigned you a new IP address.** If this is the case, head back to Google, find out your IP address again and try connecting with the same username and password as before. Note, every time you reset your router you will be assigned a new IP address by your ISP and you need top update your FTP connection settings accordingly.



  * **Your username and password information may be wrong.** If this is the case FileZilla will tell you that either the username or password was wrong so it is pretty easy to track down this problem. Head back to your local NAS web interface and change the user settings accordingly!



  * **FileZilla is unable to connect.** There may be a problem with your port forwarding configuration. Read my previous guide, “[Setting up port forwarding on a Virgin Media SuperHub with a Zyxel NSA310 NAS](http://joegardiner.co.uk/external-ftp-nas-with-the-zyxel-nsa310-media-storage-and-a-super-hub/)“, and carefully go over all the steps again. If you still have problems leave a comment on either guide and I will try to help.



  * **FileZilla very quickly fails to connect.** This is likely to be a firewall issue on the computer you are attempting to connect with. Do a quick Google search for, “opening FTP outbound on a <_insert operating system here_> firewall” and follow the instruction.






Good luck, and if you have any problems (or just want to say thanks ;)) leave a comment!
