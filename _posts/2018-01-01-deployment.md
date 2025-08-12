---
title:  "Deployment Automation"
date:   2018-01-01 19:04:23
categories: [devops]
tags: [devops]
---

Couple months back I was asked to look into deployment process of a software
developed using zope, the open source web framework for python. Deployment of
this product is a quite complex process. The complete installation of this
product on 6 servers takes about 5 days. It has 34 components to deploy.

Product is deployed using puppet master and puppet agent setup.

The puppet master runs on CentOS 6.8. This server has 8 cores and 8G memory.
Puppet agent nodes run Windows 2012 R2. Deployment process involves setting up
puppet master, puppet agents, prepare configuration files, extracting
artifacts. Then log on to each windows machine to run start deploy each
component.

This process is fine if there are 5 to 6 servers. But it will not scale up and
becomes laborious work as number of servers increase. Growing business needs
more efficient, robust and scalable product deployment solution.

Another challenge is to reduce deployment time and make it easy to deploy.

I started analyzing each process to understand pain area. I quickly learnt
that activities like setting up puppet master, puppet agents, prepare
configuration files, extract artifacts are all manual processes. After puppet
infrastructure setup, deployment engineer manually logs on to each server
using tool like remote desktop to setup puppet agents.

First , I decided to automate the manual processes. But I was told not to
introduce any new technology and tools unless absolutely necessary. Since it
is a python shop, I have chosen python for scripting, Jenkins for deployment
automation and nginx is used as local package server.

I chose Jenkins to run slaves on each puppet agent node. Also I automated
installation and setup puppet master and nginx. Jenkins job will pull the
nginx, puppet master software and install them.

But setting up puppet agent and remote execution, I have to install jenkins on
each windows server. Admin has to log on to each windows server at least once
to setup jenkins slaves. To avoid this I chose to use winrm. Configuration is
simple and done remotely. Pywinrm helped remote login to windows. This allowed
me to download puppet agent software to each windows server, install them and
handshake with puppet master remotely. Puppet agent setup and configuration
also programmed to run in parallel. Python multiprocessing module was used to
achieve this. This save good amount of time and effort. This whole activity
was talking about 6 to 8 hours for an expert deployment engineer. Setting up
of puppet agent on 6 servers has been reduced now to 3 mins.

For puppet master, yaml is used for configuration. Editing yaml was another
area of concern. Only experienced deployment engineer used to edit those huge
configuration files. I decided to generate these configuration file using
spreadsheet, so that editing is simple and syntax can be verified as they are
generated.

Now a job need to simplify yaml and puppet config generation. I captured
information like servers and component mapping in a spreadsheet to generate
yaml and puppet config needed for puppet. Thus editing spreadsheet is made
easy and this spreadsheet is read using xlrd module. This job will now
generate yaml and puppet config file in about few minutes.

Now I had to extract artifacts. In the manual process, artifact was extracted
just before starting deployment. With concurrent process, this process is
reduced to 10 minutes to extract all 34 components.

I grouped independent components and dependent and sub dependent components. I
installed all independent components in parallel and then dependent components
and worked on this order of installation and generated groups.

At this time everything is setup to start installing product components using
pywinrm.

Jenkins job started, all processes spawned, logged onto windows as expected.
Almost in first two minutes seen component started getting deployed. For my
great surprise, all components deployed and application has come up in about 5
hours.

I did many testing, everything is working as expected and installation time is
reduced by about 90%. This whole process is greatly simplified. Now anyone can
edit spreadsheet to change configurations and install product.

With this I will have to look for optimization now. I will have to optimize
database setup, zope setup.
