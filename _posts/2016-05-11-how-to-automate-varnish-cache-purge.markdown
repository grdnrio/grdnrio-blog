---
layout: post
title: Automate Varnish cache purges
description: A useful technique for automating content publishing
date: 2016-05-11
tags: [linux]
comments: true
share: true
---

This guide shows you how to use the varnishadm command in your system crontab to automate Varnish cache purging.

Access your server as root or switch to the root user. If you installed Varnish from a repo you will have all the Varnish utility commands installed as well. You need to make sure that varnishadm is installed so run the command now. You will get an output asking you for switches and arguments. This is correct.
    
    `varnishadm usage: varnishadm [-t timeout] [-S secretfile] -T [address]:port command [...]`

Before going any further you may wish to read over the varnishadm manual pages, just to familiarise yourself with the command.  
    
    `man varnishadm`

As you can see from the man page you need to specify the host and secret file for issuing varnishadm commands. To purge the cache on local host you can use the following command.  
   
    `varnishadm -T localhost:6082 -S /etc/varnish/secret url.purge .`

Let’s break this command apart.  
   
    `varnishadm -T localhost:6082`

Of course we’re using the varnishadm command. Next we define the host, look down the page for remote purges, for now as this is a local Varnish, localhost is fine. Finally we state the port that Varnish is listening on. You can check the port using the netstat command.  
    
    `netstat -l`

Look for an unrecognised TCP command.

Active Internet connections (only servers) **tcp 0 0 localhost:6082 _:_ LISTEN**

Next we have the path to the secret authentication file.  
    
    `-S /etc/varnish/secret`

It is very unlikely that you will have to alter this section.

Finally we have the actual Varnish purge command. Again you won’t need to change this.  
    
    `url.purge .

Before adding the full command to the crontab to automate the task, run it in your terminal. You should see the command executing correctly without throwing any errors. If a connection error does show, check your port number again with netstat.

If the command ran correctly you can now add it to your crontab. Copy the command then open the crontab editor (vim) with the following command.  
    
    `crontab -e`

Now press **i** to enter **INSERT** mode. You can now paste the command into the crontab.

You need to choose how often to execute this command. The options for setting cron execution times are the subject of another guide so either Google it, or search this site for instructions. In the mean time (this is an unrealistic figure) let’s set the cron to run every minute. The cron tab entry will look like this.  
   
    `* * * * * varnishadm -T localhost:6082 -S /etc/varnish/secret url.purge .

Now press **Escape**, then type **:wq**. This will write the changes and then quit from crontab editor.

That’s it, you’re done. You can stop here if you’re running a local Varnish. If not and you wish to issue cache purge commands from a remote host then keep reading.

## Remote purge commands

In order to allow remote connection you need to edit the Varnish config file on your server. Use whichever text editor you prefer.  
   
    `vim /etc/varnish/default.vcl`

You need to copy and paste the following into the default.vcl file.  
    
    `acl purge { "*server IP*"; "*123.123.123.123*"; }`

You need to update the server IP to match the IP of the remote server you will be issuing purge commands from.

Write and quit the changes and reload the config.  
    
    `service varnish reload`

Now log in to the server you will be issuing commands from. We’re going to edit the crontab again using the same command as before. The only difference is that we are going to edit the host in the varnishadm command. So, in the crontab (with your own timing settings) paste the following  
    
    `varnishadm -T *varnish-server-IP*:*port-from-netstat* -S /etc/varnish/secret url.purge .`

As you can see we have changed the IP and port to those of the target Varnish server. The rest of the command is the same.

If you have any problems or questions about this guide leave a comment and I will do my best to help you.
