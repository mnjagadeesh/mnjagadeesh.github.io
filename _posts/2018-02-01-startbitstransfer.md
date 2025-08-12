---
title:  "Welcome to Jekyll!"
date:   2018-02-01 16:04:23
categories: [devops]
tags: [devops]
---
8 Apr 2018 • on [devops](/tags/#devops)

# Start-BitsTransfer: Tool that downloads very quickly!!

I was working on a puppet module that downloads a file of size over 3GB,
unzips it and update database.Puppet master setup is on CentOS 6.8 and agent
running on Windows 2012R2. I used nginx to serve files. As usual, I used
puppet protocol to download file.

This setup was working as expected. The whole process use to complet in about
50 minutes.

After few days, I received a file size of 5GB. For this size, may times,
puppet was failing to download it. I started evaluating best available tools.
First in the list was powershell’s curl. Curl was better but not quite
satisfactory. While looking for some of the better utilities, I stumbled upon
MicroSoft’s new utility called ‘Start-BitsTransfer’. Its syntax is very simple
and easy to use. Syntax:

D:> Start-BitsTransfer -Source “http://server.localtion/file.txt” \
-Destination “D:\install_folder\file.txt”

With Start-BitsTransfer download time has come down to 4 minutes. I am now
using Start-BitsTransfer extensively to improve download times. Well Done
Microsoft.
