---
layout: post
title: Installing .NET 4.6.1 on Windows Server 2012 R2 with Chef
description: How to manage you .NET version on Windows Server with Chef
date: 2016-07-20
tags: [chef, windows]
comments: true
share: true
---

Recently I was lucky enough to take part in a 'hackathon' with Chef, Microsoft and a partner company based in Norway. It was a great week working with some amazing people!




One of the challenges we faced was automating the installation of .NET 4.6.1; a requirement of the application that was the focus of the week long event. We struggled with automatically determining whether the package should be installed on the system, and then triggering a reboot in the correct order to allow IIS configuration to complete.





Here's the [Windows package](https://docs.chef.io/resource_windows_package.html) resource we used to install .NET originally.




    
    <code class="language-bash">  package '.NET 4.6.1' do
        source 'https://download.microsoft.com/download/E/4/1/E4173890-A24A-4936-9FC9-AF930FE3FA40/NDP461-KB3102436-x86-x64-AllOS-ENU.exe'
        installer_type :custom
        action :install
        returns [0, 3010]
        options '/norestart /passive'
        timeout 3000
      end
    </code>





As you can see we needed to set the installer type to `:custom` because we were using a .exe package. In addition to this we passed some options to prevent an automatic reboot interrupting the Chef run, and to prevent user dialogues from interrupting the installation.





This worked, but running `chef-client` again would re-trigger the installation. Additionally, we had issues with IIS configuration (a different recipe) due to .NET installation being our first task. The flagged reboot prevented IIS features from fully enabling.





We decided to push the reboot out to the [reboot resource](https://docs.chef.io/resource_reboot.html) with a notification from the Windows package resource.




    
    <code class="language-bash">reboot '.Net Install' do  
      reason 'Need to reboot after .NET installation'
      action :nothing
    end
    
    if version_arr[6][:data] != 394_271  
      package '.NET 4.6.1' do
        source 'https://download.microsoft.com/download/E/4/1/E4173890-A24A-4936-9FC9-AF930FE3FA40/NDP461-KB3102436-x86-x64-AllOS-ENU.exe'
        installer_type :custom
        action :install
        returns [0, 3010]
        options '/norestart /passive'
        notifies :request_reboot, 'reboot[.Net Install]', :immediately
        timeout 3000
      end
    </code>





We also changed the recipe order in `default.rb` to make .NET installation the last task; this fixed our reboot flag issue with IIS.





With this working we could now trigger a reboot from the successful installation of .NET as the last task in the `chef-client` run. Using `request_reboot` in the Reboot resource ensured a complete run before the machine going down.





We still had the problem of idempotency. Every time we converged the .NET installation kicked off, even if it was already installed at the correct version. To resolve this we added some logic. We settled on checking the registry for the [build release number](https://msdn.microsoft.com/en-us/library/hh925568(v=vs.110).aspx) (matching 4.6.1) to either skip or trigger the windows_package resource.




    
    <code class="language-bash">reboot '.Net Install' do  
      reason 'Need to reboot after .NET installation'
      action :nothing
    end
    
    version_arr = registry_get_values('HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full', :x86_64)
    
    if version_arr[6][:data] != 394_271  
      package '.NET 4.6.1' do
        source 'https://download.microsoft.com/download/E/4/1/E4173890-A24A-4936-9FC9-AF930FE3FA40/NDP461-KB3102436-x86-x64-AllOS-ENU.exe'
        installer_type :custom
        action :install
        returns [0, 3010]
        options '/norestart /passive'
        notifies :request_reboot, 'reboot[.Net Install]', :immediately
        timeout 3000
      end
    end  
    </code>





Here's the complete, working code.
