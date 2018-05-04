---
layout: post
title: Cloning a hard drive in Windows 7
description: What's the easiest way to move to a new device or create a complete disk backup?
date: 2012-07-28
tags: [windows]
comments: true
share: true
---

Cloning a hard drive is an especially useful thing to do if you wish to replace your existing hard drive. I recently did this and upgraded from my 7.2K 1TB Western Digital drive to a [120GB OCZ Agility 3 SSD](http://www.ebuyer.com/268244-ocz-120gb-agility-3-ssd-agt3-25sat3-120g-agt3-25sat3-120g) (it was absolutely worth it by the way!).

This is how I did it…

I used a really useful tool called Reflect. It is quite tricky to find the free version of it these days, but to help you out here is a link: [Macrium Reflect Free Edition](http://download.cnet.com/Macrium-Reflect-Free/3000-2242_4-10845728.html?part=dl-&subj=dl&tag=button). I’ll do my best to keep the link updated…

When the download is complete install the software. I tested for viruses and it is clean. It also seemed to run very smoothly on my Windows 7 workstation. Fire the program up and you’ll see that it scans your drives to work what you currently have connected to your computer.

Now you need to connect the drive which you wish to clone to (the new drive) to your computer. You could do this by opening your computer up and connecting it internally, or you could use a USB external drive mounter, something like this: [Startech External Hard Drive Dock](http://www.ebuyer.com/166418-startech-external-esata-usb-to-sata-hard-drive-dock-hi-speed-satdocku2egb). Either way attach the device and power your computer back on, if you shut it down.

When it comes back open up My Computer to see if the new drive is detected. If it isn’t you may need to format it and create a simple volume. To do this follow the guide, “Creating a simple volume on a new hard drive in Windows 7”. If the drive is there you can go back to the Reflect program you installed and open it up.

Your hard drives, including the new one, will be listed when the program opens up. Left click on the drive you wish to clone, then click on the **Clone this disk** button underneath the drive. This will start up the disk cloning wizard.

[![](https://images.grdnr.io/2012/04/Wizard.png)](https://images.grdnr.io/2012/04/Wizard.png)

Now select a destination drive to clone to, this will be your new drive. Choose your new drive from the list that appears after clicking the ‘Select a disk to clone to’ link. When the drive is selected click Next. This will begin the cloning process. Follow the rest of the options through to complete the disk cloning.

Depending on the size of the drive and how it is connected to your computer it make take around an hour.

You can now open up My Computer and view the contents of the drive. It should be exactly the same as the source drive that you cloned. Now if you are cloning a drive for backups you can stop here, however if you are cloning a drive in order to replace an old drive, you will need to change drive letters in order for your installed programs to continue to work correctly running from the cloned drive.

Go to Start button -> All Programs -> Administrative Tools -> Computer Management. Then Storage, and Disk Management.

[![](https://images.grdnr.io/2012/04/disk-clone-1.png)](https://images.grdnr.io/2012/04/disk-clone-1.png)

Right click on the blue volume block of the disk you cloned from (the source) and from the options menu select **Change drive letter and paths**.

Now click on the **Change** button.

[![](https://images.grdnr.io/2012/04/Change-letter1.png)](https://images.grdnr.io/2012/04/Change-letter1.png)

Then choose the new drive letter. I went with X for clarity when doing this.

[![](https://images.grdnr.io/2012/04/Change-letter2.png)](https://images.grdnr.io/2012/04/Change-letter2.png)

Now repeat the process for your newly created clone drive (destination drive) but choose the original drive letter of the destination drive, probably C or D.

The Disk Manager will ask you to restart your computer for changes to take effect; do this now. When the computer comes back online you should see that the drive letters have swapped over. You can shut your computer down again and remove the old drive from inside the computer.

Now when you turn the computer back on again you will be able to run any programs and access data that may have been on a secondary drive without any problems. A good example for me was Steam; games were installed on my D drive which I cloned to a new SSD.
