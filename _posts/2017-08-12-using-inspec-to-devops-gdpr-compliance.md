---
layout: post
title:  "Using InSpec to DevOps GDPR compliance"
description: "How can you ease the process of achieving GDPR Compliance by using InSpec?"
date:   2017-08-12 12:43:57 +0100
tags: [inspec, security]
comments: true
share: true
---

![EU Flag](https://images.grdnr.io/2017/EU-flag.jpg)

Have you heard of GDPR? It stands for the [General Data Protection Regulation](http://www.eugdpr.org/). It's an update to the original data protection regulations from 1995 and reflects the new ways that we store and process personal data in an increasingly digital society. It impacts any business that stores and processes the personal data of EU citizens (whether EU based or not), and introduces a number of requirements relating to access to data, breach notifications, and the right to be forgotten.

Should an organisation subject to GDPR fail to meet the requirements and fail an audit, it will be subject to financial penalties. These can be up to &euro;20 million, or 4% of global annual turnover, whichever is greater.

Whilst GDPR undoubtedly introduces a number of new requirements for an organisation as a whole, the specific technical controls for an IT function are not defined beyond a baseline of best practises. What is mandated however is the idea of 'Privacy by Design'. GDPR requires that all new projects involving the creation of new data processes should have privacy considerations included in the design stage and implemented at the core. This introduces a range of challenges relating to auditing at scale, audit schedules and, most importantly, audit integration with the processes being designed.

## Compliance as code
If we're building new IT processes, often redefining existing processes such as infrastructure deployment in code (Terraform, CFT, ARM templates etc), we can do the same with our compliance controls. InSpec allows us to do this.

It's an open source project from Chef; very high level and readable, reflects the security control structure many InfoSec orgs are used to, and be executed against systems in a number of ways.

If added a few examples that are relevant to GDPR, bearing in mind the requirement for data privacy / security.

### Examples
Here's an example for some baseline MySQL hardening.

{% gist dbb53c2f8884bf55c1282e1374fbb257 %}

Another example, this time of workstation hardening.

{% gist d4709f1c09200c936dceaf87562fe756 %}

Finally an example of checking firewall config.

{% gist 6d0905fb6b75e227d887ca58b7a8c618 %}

### Profiles
InSpec lets us collect these code blocks, each representing a security control, into profiles. The profiles align to our business or regulatory requirements. For example, we can build a profile that combines GDPR with our business' baseline security configuration- think domain joining, password length or system entropy.

You can check out a couple of baseline on the [Dev-Sec](https://github.com/dev-sec/) GitHub org.

* [Windows baseline profile](https://github.com/dev-sec/windows-baseline)
* [Linux baseline profile](https://github.com/dev-sec/linux-baseline)

I'll add a link to a GDPR profile when it's available.

## InSpec Automation
InSpec profiles can be executed against a target system in a number of ways. Whichever method you decide upon requires the installation of the [InSpec Gem](https://rubygems.org/gems/inspec/versions/0.9.9) (on the target system or your workstation if you decide to go with remote execution).

It's worth noting the [Audit cookbook](https://blog.chef.io/2016/11/09/the-audit-cookbook-a-how-to/) that can be executed via the Chef agent. This forms a natural part of a Chef based approach to system configuration. However, you may also wih to use the gem standalone, or execute target systems from the [InSpec CLI](https://www.inspec.io/docs/reference/cli/). More important than the execution method is the use case.

### Pipeline execution
A non-Chef based pipeline would suit the local execution method. You could pull a profile hosted on GitHub or similar to a target system and run InSpec as follows, or execute against a remote machine from an admin node or similar. This certainly demonstrates the GDPR requirement for privacy by design for any application deployment pipeline.

{% highlight bash %}
inspec exec --host=1.2.3.4 --profiles-path=/etc/inspec/profile/my-GDPR-profile --format=json
{% endhighlight %}

Note the ouptut format. This is really useful if you want to forward results to something like Splunk.

### Infrastructure deployment
Automated infrastructure deployment, on cloud or on-prem, offers a great opportunity to build GDPR auditing in from the start. There are many methods available, but user-data is common. We can used user-data to execute InSpec, similar to the above, or bootstrap the system with Chef for agent based execution. The following assumes the latter.

User data to bootstrap is available on the Chef Docs.

Please note that you'll need to set the audit cookbook in the node's run-list, either directly or with a base role for example.

Once bootstrapped you'll need to set some attributes for the Audit cookbook to enable audit scanning.

{% gist f277094fdcfd8f62cb828b7a9132c228 %}

The above example could be set in a role or via the UI if you prefer. You'll see we're setting a profile located on GitHub. This will be downloaded to the target machine as part of the Chef run. Data is being reported directly to the Chef Automate platform.

## Further information
There are many more examples for InSpec and GDPR scanning, such as container based scenarios or as part of a server migration process. I'll continue to add blogs with more detail as GDPR technical controls become clearer and integration points develop.

In the meantime I highly recommend the [Chef InSpec tutorials](https://learn.chef.io/tracks/compliance-automation/) for further learning.
