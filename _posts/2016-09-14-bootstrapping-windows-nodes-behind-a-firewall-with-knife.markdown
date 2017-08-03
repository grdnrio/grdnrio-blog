---
layout: post
title: Bootstrapping Windows nodes behind a firewall with Knife
description: Using proxy settings to bootstrap firewalled nodes
date: 2016-09-14
tags: [chef, windows]
comments: true
share: true
---


Using knife to bootstrap a node to be managed with Chef is a fundamental part of the Chef workflow, especially for dev and test purposes. When you bootstrap a node you are preparing the node to communicate with the Chef Server so it can download the cookbooks and recipes you have defined in its run-list, and eventually match the state you have defined in your Chef code.



    
    <code class="language-bash">knife bootstrap windows winrm ADDRESS --winrm-user USER --winrm-password 'PASSWORD' --node-name node1 --run-list 'recipe[learn_chef_iis]'  
    </code>





As part of the bootstrapping process for Windows the chef-client package is retrieved from the chef.io website. You can see this in the process output.




    
    <code class="language-bash">54.171.10.153 C:UsersAdministrator>goto install  
    54.171.10.153 Checking for existing downloaded package at "C:UsersADMINI~1AppDataLocalTempchef-client-latest.msi"  
    54.171.10.153 No existing downloaded packages to delete.  
    54.171.10.153 Attempting to download client package using PowerShell if available...  
    54.171.10.153 powershell.exe -ExecutionPolicy Unrestricted -NoProfile -NonInteractive -File  C:chefwget.ps1 "https://www.chef.io/chef/download?p=windows&pv=2012&m=x86_64&DownloadContext=PowerShell&v=12" "C:UsersADMINI~1AppDataLocalTempchef-client-latest.msi"  
    54.171.10.153 Download via PowerShell succeeded.  
    54.171.10.153 Installing downloaded client package...  
    54.171.10.153  
    54.171.10.153 C:UsersAdministrator>msiexec /qn /log "C:UsersADMINI~1AppDataLocalTempchef-client-msi7958.log" /i "C:UsersADMINI~1AppDataLocalTempchef-client-latest.msi"  
    54.171.10.153 Successfully installed Chef Client package.  
    54.171.10.153 Installation completed successfully  
    </code>





If working in a locked down environment, perhaps behind a firewall, this can be problematic. If your node is unable to retrieve a package from the Internet the bootstrapping process will fail.





The work around is to use a (currently) undocumented argument in your bootstrap command.





`--msi-url`





This argument will accept a remote location as well as a local system path. This means you can use an internal package hosting service of some kind, or reference the package location on the node's filesystem; perhaps baked into your images.




    
    <code class="language-bash">knife bootstrap windows winrm ADDRESS --winrm-user USER --winrm-password 'PASSWORD' --node-name node1 --msi-url C:/tmp/chef-client.msi  
    </code>





Voila, your locked down instance is bootstrapped.
