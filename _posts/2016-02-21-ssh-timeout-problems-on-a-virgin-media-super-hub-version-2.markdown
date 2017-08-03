---
layout: post
title: SSH Timeout Problems on a Virgin Media Super Hub version 2
date: 2016-02-21
tags: [linux, ssh]
comments: true
share: true
---

A commonly reported problem with the Virgin Media SuperHub is that an SSH connection will timeout. This is apparently due to the device having a very small amount of RAM leading to connection details being dropped.

If you are using a Mac or a Linux computer there is a solution, as follows.

First of all bring up a terminal on your respective machine. Then change to you .ssh directory.  
    
`cd /Users/username/.ssh/`

Now create a local config file either using touch or just go straight into vim.  
    
`vim config`

When the editor opens copy and paste the following into it. Remember to press **i** to enter insert mode in vim.  
    
`# Site-wide defaults for various options Host * ServerAliveCountMax 600 ServerAliveInterval 10`

Then save and exit. Hit escape to exit insert mode in vim.  
    
`:wq`

Now it’s probably a good idea to reboot your computer. Next time you try logging in with SSH, leave the terminal idle for a while and see if it times out. You should be fine! I’ve left my terminal open for hours with the connection remaining active.
