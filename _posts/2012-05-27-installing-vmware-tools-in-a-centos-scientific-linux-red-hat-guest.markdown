---
layout: post
title: Installing VMware Tools in a Centos / Scientific Linux / Red Hat guest
date: 2012-05-27
tags: [linux]
comments: true
share: true
---

Installing VMware tools in a rpm based guest can sometimes be tricky. Here is a quick and dirty guide to help you install VMware tools and get the most out of your Linux guest.

#### Getting Started
Of course the first thing you will need is a VMware software product (depending on your OS) capable of running guests. I use a Mac Book Air circa 2011 running Lion so I have VMware Fusion installed. You can get it from here if you also run a Mac: [VMware Fusion](http://www.vmware.com/uk/products/desktop_virtualization/fusion/overview.html). If you’re running Windows or Linux have a look at their [products page](http://www.vmware.com/uk/products.html) to select the best option for you.

Surprisingly you will then need to install a RPM based Linux guest. How to do that goes beyond the scope of this guide, I will presume you are able to do this. I may write a guide at a later date, but their are loads of tutorials online and in the VMware help guides.

* * *

Boot up your Linux VM and wait until you get to a stable desktop, if you are using one. If you installed your distro without a desktop environment you can still use the following commands (although you probably won’t need this guide if you prefer a CLI only OS!).

When the OS is booted, have a look at the top of your VMware app window. There should be a toolbar, you want the tab called Virtual Machine. In this menu select ‘**Install VMware Tools**‘. When you click this it will mount an ISO with the VMware tools package on it.

[![VMware menu](https://images.grdnr.io/2012/03/Screen-Shot-2012-03-21-at-20.05.05.png)](https://images.grdnr.io/2012/03/Screen-Shot-2012-03-21-at-20.05.05.png)

First switch to root.

`su -`

You need to set the CD at a mount point so that you can copy over the VMware Tools files.

`mount /dev/cdrom /mnt`

Now copy the files to your Desktop.

`cp /mnt/VMwareTools-* /home/user/Desktop`

Remember to use _Tab_ to auto complete the VMware Tools version, and change _user_ with your own username.

Change to your Desktop so that you can extract the VMware Tools archive.

`cd /home/user/Desktop`

Now extract the archive.

`tar -xzvf VMwareTools-*`

Change to the extracted folder in order to run the VMware Tools installer.

`cd vmware-tools-distrib/`

Run the installer.

`./vmware-install.pl`

You will now be asked a lot of questions by the installer. Generally you can go with the default settings by pressing _Enter_ and everything will install correctly. The advanced settings of the installer are beyond the scope of this guide!

[![VMware Tools automated installer](https://images.grdnr.io/2012/03/Screen-Shot-2012-03-22-at-18.27.29.png)](https://images.grdnr.io/2012/03/Screen-Shot-2012-03-22-at-18.27.29.png)

When the installer completes reboot your VM, either using the VMware Player / Fusion / Workstation settings, or with the following command.

`reboot`

When your Linux VM comes back online you will have all the advanced features of VMware tools such as directory sharing, clipboard sharing, graphics adapter interfacing, automatic resolution adjustment etc.

Finally to remove those left over files on your Desktop. You will need to be root again.

`su -`

Change to the Desktop.

`cd /home/user/Desktop/`

Then delete the files.

`rm -f -R VMwareTools-* vmware-tools-distrib/`

And you’re finished! Enjoy your Linux VM with VMware Tools!
