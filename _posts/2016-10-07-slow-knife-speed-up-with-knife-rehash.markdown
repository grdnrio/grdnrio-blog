---
layout: post
title: Slow Knife? Speed up with Knife rehash
description: Speed up the performance of the knife CLI on Windows using the rehash command
date: 2016-10-07
tags: [chef, windows]
comments: true
share: true
---

The Chef DK includes a tool called Knife. This is primarily used for interaction with remote resources in Chef, such as the Chef Server and Chef managed nodes, but can be used for managing other remote systems too. Here's the description from the Chef Docs.
 
> knife is a command-line tool that provides an interface between a local chef-repo and the Chef server.

[https://docs.chef.io/knife.html](https://docs.chef.io/knife.html)

It's a great tool and a vital part of any Chef administration tasks. Unfortunately Ruby on Windows is slower than Linux and so Windows users sometimes experience performance issues. Check out the result of the following running on my top-spec Ultrabook running Windows 10:
    
    ```powershell
    C:UsersjgardRepositorieschef-repo> Measure-Command {knife node list}
    
    Days              : 0  
    Hours             : 0  
    Minutes           : 0  
    Seconds           : 6  
    Milliseconds      : 312  
    Ticks             : 63120307  
    TotalDays         : 7.30559108796296E-05  
    TotalHours        : 0.00175334186111111  
    TotalMinutes      : 0.105200511666667  
    TotalSeconds      : 6.3120307  
    TotalMilliseconds : 6312.0307  
    ```

A wait of 6 seconds every time I want to run the commonly used `knife node list` is not ideal. Fortunately a knife command that comes with the Chef DK by default is available to address this issue.

Using `knife rehash` we can create a cache of the local knife sub commands on disk. When knife runs it no longer builds a tree of available sub commands, which reduces execution time.
    
    ```powershell
    C:UsersjgardRepositorieschef-repo> knife rehash  
    Using knife-rehash will speed up knife's load time by caching the location of subcommands on disk.  
    However, you will need to update the cache by running `knife rehash` anytime you install a new knife plugin.  
    Knife subcommands are cached in C:/Users/jgard/.chef/plugin_manifest.json. Delete this file to disable the caching.  
    ```

After creating the cache the `knife node list` command performance is greatly improved.
   
    ```powershell
    C:UsersjgardRepositorieschef-repo> Measure-Command {knife node list}
        
    Days              : 0  
    Hours             : 0  
    Minutes           : 0  
    Seconds           : 2  
    Milliseconds      : 954  
    Ticks             : 29545646  
    TotalDays         : 3.4196349537037E-05  
    TotalHours        : 0.000820712388888889  
    TotalMinutes      : 0.0492427433333333  
    TotalSeconds      : 2.9545646  
    TotalMilliseconds : 2954.5646  
    ```
There are some caveats:

  * If you install a new Knife gem or plugin you need to re-run `knife rehash`.
  * The same applies for a chef DK update- don't forget knife rehash!
  * If you get strange output with Knife it's worth trying a `knife rehash` before debugging further.

Finally, if you want to stop using the cache just delete the JSON file. On Windows it's here: `C:/Users/<username>/.chef/plugin_manifest.json`
