---
layout: post
title:  "Security scanning Docker containers with InSpec"
description: "Using InSpec to scan and verify the state of a Docker container"
date:   2017-08-14 13:07:11 +0100
tags: [inspec, security, docker]
comments: true
share: true
---

Docker makes running containers incredibly simple, a big reason for its popularity. I can quickly and easily run an Nginx container on my workstation, whether Mac, Windows or Linux based.

`docker container run --publish 80:80 --detach --name nginx nginx`

And as if my magic...

![Nginx local host](https://images.grdnr.io/2017/docker-nginx.gif)

The certified images from Docker are great, and if you use Docker Cloud you can push the images through a pipeline that executes security / vulnerability scanning on the image. However, there are plenty of images in the registry from community contributors - how can you verify them? What sit he image contains software that needs to be patched?

We can use InSpec, the compliance testing project from Chef, to verify the state of our Docker images against a security or compliance baseline. You can read more about the InSpec language and the resources available for writing tests on the [homepage](https://www.inspec.io/), or in [my blog](https://grdnr.io/2016-10-22/inspec-and-chef-compliance-as-code/) on the subject.

**Please note, at the moment this will not work on Windows containers**

## InSpec setup
You can install InSpec as part of the ChefDK or by grabbing the executable from the [downloads page](https://downloads.chef.io/inspec/1.33.1).

Once installed, reload your terminal session and you should have the inspec CLI in your path.

{% highlight bash %}
C:\Users\jgard> inspec --help
Commands:
  inspec archive PATH                # archive a profile to tar.gz (default) ...
  inspec artifact SUBCOMMAND ...     # Sign, verify and install artifacts
  inspec check PATH                  # verify all tests at the specified PATH
  inspec compliance SUBCOMMAND ...   # Chef Compliance commands
  inspec detect                      # detect the target OS
  inspec env                         # Output shell-appropriate completion co...
  inspec exec PATHS                  # run all test files at the specified PATH.
  inspec habitat SUBCOMMAND ...      # Commands for InSpec + Habitat Integration
  inspec help [COMMAND]              # Describe available commands or one spe...
  inspec init TEMPLATE ...           # Scaffolds a new project
  inspec json PATH                   # read all tests in PATH and generate a ...
  inspec shell                       # open an interactive debugging shell
  inspec supermarket SUBCOMMAND ...  # Supermarket commands
  inspec vendor PATH                 # Download all dependencies and generate...
  inspec version                     # prints the version of this tool

Options:
  l, [--log-level=LOG_LEVEL]         # Set the log level: info (default), debug, warn, error
      [--log-location=LOG_LOCATION]  # Location to send diagnostic log messages to. (default: STDOUT or STDERR)
      [--diagnose], [--no-diagnose]  # Show diagnostics (versions, configurations)
{% endhighlight %}

Using the inspec CLI, we can execute scans against local or remote machines. For example the following will execute a profile against the local machine.

{% highlight bash %}
inspec exec /path/tp/profile/linux_baseline
{% endhighlight %}

... and this can be used for remote machines. 

{% highlight bash %}
inspec exec /path/tp/profile/linux_baseline -t ssh://1.2.3.4
{% endhighlight %}

Note we're not setting any additional options, so check the `inspec exec` help for more information.

Both examples assume we have a profile on our local machine for scanning purposes. There are loads of open source profiles available on the [Dev-Sec project's GitHub page](https://github.com/dev-sec). You can clone a profile or just grab the archive. The key thing is that the profile follows the skeleton format so the InSpec CLI can interpret it correctly.

## Scanning a container
I'm going to demonstrate a simple scan against the latest Windows Server Core image from the registry. First let's run the container.

`docker container run --detach -i --name ubuntu ubuntu`

Check it's running.

{% highlight bash %}
root@ip-172-31-47-31:~/profiles# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
6242a0d510c1        ubuntu              "/bin/bash"         9 minutes ago       Up 9 minutes                            ubuntu
{% endhighlight %}

Now let's grab an InSpec profile to run against this container.

{% highlight bash %}
root@ip-172-31-47-31:~/profiles git clone https://github.com/dev-sec/linux-baseline.git
Cloning into 'windows-baseline'...
remote: Counting objects: 206, done.
remote: Total 206 (delta 0), reused 0 (delta 0), pack-reused 206R
Receiving objects: 100% (206/206), 39.44 KiB | 0 bytes/s, done.
Resolving deltas: 100% (100/100), done.
{% endhighlight %}

Using the InSpec CLI I can now run the profile against the Docker container passing in the path to the Linux baseline I just cloned and setting the Docker container ID as a target.

`inspec exec linux-baseline -t docker://6242a0d510c1`

Here's an example of an output against the official Ubuntu image.

{% highlight bash %}
--- SOME TEST OUPUT ---

  ✔  sysctl-31a: Secure Core Dumps - dump settings
     ✔  Kernel Parameter fs.suid_dumpable value should cmp == /(0|2)/
  ×  sysctl-31b: Secure Core Dumps - dump path (expected "|/usr/share/apport/apport %p %s %c %P" to match /^\/.*/
     Diff:
     @@ -1,2 +1,2 @@
     -/^\/.*/
     +"|/usr/share/apport/apport %p %s %c %P"
     )
     ×  Kernel Parameter kernel.core_pattern value should match /^\/.*/
     expected "|/usr/share/apport/apport %p %s %c %P" to match /^\/.*/
     Diff:
     @@ -1,2 +1,2 @@
     -/^\/.*/
     +"|/usr/share/apport/apport %p %s %c %P"

  ✔  sysctl-32: kernel.randomize_va_space
     ✔  Kernel Parameter kernel.randomize_va_space value should eq 2
  ✔  sysctl-33: CPU No execution Flag or Kernel ExecShield
     ✔  /proc/cpuinfo Flags should include NX

Profile Summary: 22 successful, 30 failures, 1 skipped
Test Summary: 62 successful, 56 failures, 1 skipped
{% endhighlight %}

## Wrapping up
In the example above I'm using a linux-baseline profile meant for complete Linux OS', not an Ubuntu based container. Having said that the principle is still incredibly relevant in a container based workload environment.

InSpec allows us to test the output of a Docker container build, essentially define integration tests for containers... it's just a matter of designing the tests!

Using the InSpec CLI this can simply for part of a CI/CD pipeline, with a build node calling the InSpec CLI against a dynamic Docker target (container ID).
