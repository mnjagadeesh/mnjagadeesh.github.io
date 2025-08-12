---
title:  "Build a product with git Hook !!"
date:   2016-01-03 15:04:23
categories: [devops]
tags: [devops]
---
I noticed that few products have very complex building system. Even
experienced engineers spend good amount of time understanding the build
system. Based on complexity of the build system, it may take few weeks to be
productive. It might take months in some cases. Initial experience will not be
smooth for them.

On the other hand, resources are needed to create and maintaining document
about building product. It involves cost and effort.

Build team finds difficulty updating a system, making any modification, adding
any new features. Because it involves, sometime extensive, training and
documentation.

I was thinking to find an alternative, effective way. My initial idea was to
choose popular tool that is almost known to everyone. Just to avoid additional
effort of training.

My obvious first choice was to make use of Jenkins. But Jenkins needs JRE and
a network port to run. Again, not sure if everyone can use Jenkins. I have
seen few folks who are reluctant to use it.

During this time, I was fine-tuning git hooks. I personally use hooks for
mutt, emacs, version control systems. I got an idea if git post-checkout hook
can be used for to build the product. Idea is automating all tedious manual
steps.

To experiment this idea I chose a small project built using
[buildout][http://docs.buildout.org/]. It is a just simple CRUD application
built using python and zope and MSSQL as backend database.

This application is little complex to set up. It makes use of various third
party components like RabbitMQ, Memcache and many other python modules. The
buildout configuration is slightly complex and takes about 50 minutes to
complete build. Configuration involves setting network port, database info and
many other values. Different version of product branch use different database
version. One has to obtain this information from DB team.

First step is automated manual steps with a python script in post-checkout,
updated ~/.gitconfig to make hooks to reflect new hook on next clone. Cloned
repo of product. After successful Cloning, defaulted branch was checkedout. At
this point automagically, post-commit hook started executing. It did update
buildout configuration, executed buildout successfully. Product installation
is successful, I could see login page.

Since all steps were executed in machine time, complete build took
considerably less time.

Next step is start considering different use cases.

For a new engineer, it takes good amount of time to collect required
information needed to update build configuration.

I have created separate branch specific configuration files so that it is easy
make available up-to-date and accurate configuration. These configuration
files will be maintained by build team.

If system breaks during setup then either user has to investigate and fix
issue or wait till expert comes for rescue. I noticed that many times it takes
too long to understand root cause of the issue. Much time is spent collecting
logs and additional data about build break.

This use case is handled by adding additional logging in a file. Build
engineer can inspect this file to understand what went wrong.

This also address issue of announcements and educating users. Apart from
general usage, developers can consult manual pages for certain flags. This
reduces lot of ping-pong used to happen.

Then to make it more effective, added few auto healing techniques. So that,
known issues used to be fixed automatically. If new issue is encountered,
ticket will be filed appropriately with adequate log. Fix for this new ticket
is made as known solution for auto healing.

After few days, keeping hooks up-to-date was a challenge. Then I moved heavy
lifting part of installer to a separate script to root folder. Now, Git post
commit hook invokes this script. This set up is going well for now.

Most of the engineers are familiar with git and its configuration.

I found it simple and effective solution to solve a challenge. It is just an
attempt to make happy starting for new engineers with decreased churn.

If you get any questions, clarifications please leave a comment. I will reply.
