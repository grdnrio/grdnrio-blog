---
author: joe@grdnr.io
comments: true
date: 2016-10-07 18:21:15+00:00
layout: post
link: http://grdnr.io/install-posh-git-on-windows-10-x64/
slug: install-posh-git-on-windows-10-x64
title: Install Posh Git on Windows 10 (x64)
wordpress_id: 60
---

Posh Git is a really helpful open-source project that includes a number of scripts offering Git and Powershell integration. When installed it allows tab auto completion for common Git operations along with showing the current branch and state of files.




![poshgit_branch](http://images.grdnr.io/2016/Posh+Git+PS.PNG)





As you can see above, showing the current branch is neat.





### Installation





Let's get this bad boy installed.





Firstly you need to install Git for Windows. Head to the Git page (not GitHub) and download the correct package for your version of Windows. [https://git-scm.com/download/win](https://git-scm.com/download/win)





Once the package has downloaded, click through the installation options. I went with the Windows command prompt integration instead of the provided terminal environment.





Now you should be able to type Git into a Powershell terminal and see that options are spat back out, suggesting that Git has been installed successfully.





![Git Powershell output](http://images.grdnr.io/2016/git-output.PNG)





Now we need to edit our security settings before installing PoshGit. Make sure that you're running your Powershell environment as Administrator. If not you can right click on a Powershell shortcut and choose this option.





We're going to run a command that allows us to execute remotely signed scripts. Essentially we're switching to Developer mode instead of relying on packages signed by Microsoft or trusted developers.





Run this command now:





`Set-ExecutionPolicy RemoteSigned`





It will ask for confirmation, so say yes with `Y`.





Now we need to grab the PoshGit repo. It's time to use your shiny new Git command line tools. Before cloning the PoshGit repo it might be helpful to create a Repository or Project directory in your file system. Personally I use C:UsersJoe GardinerRepositories.





After changing to a suitable directory on your file system, run this command.





`git clone https://github.com/dahlbyk/posh-git.git`





You can view the full proect page on GitHub - [https://github.com/dahlbyk/posh-git](https://github.com/dahlbyk/posh-git)





Now change to the recently cloned PoshGit directory and run the installer script





`cd posh-git
.install.ps1`





When the installation completes the installer will tell you that you need to reset your session. Do this with the following command.





`. $PROFILE`





Voila. Post Git in your Powershell environment. One thing to note is that you need to set your Execution Policy for each Powershell environment (x86 and x64 for example).
