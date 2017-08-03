---
layout: post
title: WordPress .htaccess rules for Nginx
description: How to make WordPress permalinks work properly with Nginx web server
date: 2016-05-03
tags: [linux, wordpress]
comments: true
share: true
---

This guide shows you the equivalent .htaccess rewrite rules to get WordPress working properly with permalinks on the Nginx web server.

There is no direct equivalent for .htaccess in Nginx, so you cannot copy and past the .htaccess rules that WordPress suggests when you change your permalink settings in the dashboard. Instead you need to add the following rules to you site local configuration file.

You could put these rules in the global nginx.conf file, generally found at…  
    
    `/etc/nginx/nginx.conf`

…but it is much better practice to have a single configuration file with local overrides for each site you are hosting on your Nginx server. Not only does this lead to granular management on a site by site basis, but it also avoids issues that may occur with multiple PHP apps (Drupal, Joomla! etc) each requiring slightly different rewrite rules.

First of all either create or edit the configuration file for your site.
    
    `vim /etc/nginx/sites-available/*website-name*`
    
The standard configuration file looks something like this. This is the file used for **sudoguides.com**  




    
    <code class="language-bash">server { #Add a server_name entry for each mapped domain server_name sudoguides.com www.sudoguides.com; server_name sudoguides.co.uk www.sudoguides.co.uk; server_name sudoguides.net www.sudoguides.net; access_log /srv/www/sudoguides.com/logs/access.log; error_log /srv/www/sudoguides.com/logs/error.log; root /srv/www/sudoguides.com/public_html; location ~ .php$ { include /etc/nginx/fastcgi_params; fastcgi_pass 127.0.0.1:9000; fastcgi_index index.php; fastcgi_param SCRIPT_FILENAME /srv/www/sudoguides.com/public_html$fastcgi_script_name; } }  
    </code>





Now for a single WordPress installation (not multisite) you need to add the following to this file.  




    
    <code class="language-bash">if (!-e $request_filename) { rewrite ^.* /index.php break; }  
    </code>





For a multisite WordPress installation you need to add this.  




    
    <code class="language-bash"># Rewrite for multi site files rewrite /files/(.+)$ /wp-includes/ms-files.php?file=$1 last; # Rewrite for wordpress if (!-e $request_filename) { rewrite ^(.+)$ /index.php?q=$1 last; }
    </code>





After adding the rules remember to restart Nginx.




    
    <code class="language-bash">service nginx restart  
    </code>





Please note, after restarting Nginx you may need to reset your permalink settings in the WordPress dashboard. Just reselect the type of permalinks you want and click save again.





As usual if you have any problems leave a comment and we will do our best to help you out!
