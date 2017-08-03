---
author: joe@grdnr.io
comments: true
date: 2016-10-07 18:21:13+00:00
layout: post
link: http://grdnr.io/ssh-timeout-problems-on-a-virgin-media-super-hub-version-2/
slug: ssh-timeout-problems-on-a-virgin-media-super-hub-version-2
title: SSH Timeout Problems on a Virgin Media Super Hub version 2
wordpress_id: 56
---

A commonly reported problem with the Virgin Media SuperHub is that an SSH connection will timeout. This is apparently due to the device having a very small amount of RAM leading to connection details being dropped.




If you are using a Mac or a Linux computer there is a solution, as follows.





First of all bring up a terminal on your respective machine. Then change to you .ssh directory.  




    
    <code class="language-bash">cd /Users/username/.ssh/  
    </code>





Now create a local config file either using touch or just go straight into vim.  




    
    <code class="language-bash">vim config  
    </code>





When the editor opens copy and paste the following into it. Remember to press **i**to enter insert mode in vim.  




    
    <code class="language-bash"># Site-wide defaults for various options Host * ServerAliveCountMax 600 ServerAliveInterval 10
    </code>





Then save and exit. Hit escape to exit insert mode in vim.  




    
    <code class="language-bash">:wq
    </code>





Now it’s probably a good idea to reboot your computer. Next time you try logging in with SSH, leave the terminal idle for a while and see if it times out. You should be fine! I’ve left my terminal open for hours with the connection remaining active.
