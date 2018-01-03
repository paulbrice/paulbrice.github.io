---
layout: post
title: Removing Non-Present Adapters
date: 2012-08-01 01:10:00 -07:00
categories: networking
---
In my lab virtual machines after a migration from Hyper-V to VMware there are hardware network adapters that have been removed due to the migration with their network configurations remaining in the registry. Removing these have always been a pain, after a little searching I came across this Tech Net article for [Windows XP][external-link]. 

I tried this in Windows 2008 R2 and it worked a treat. 

Steps: 

- Go to All Programs , point to Accessories , and then click Command Prompt .
- At a command prompt, type the following command prompt; set devmgr_show_nonpresent_devices=1 
- At a command prompt, type the following command a command prompt; start devmgmt.msc 

Important: Select 'show hidden devices' on the View  menu in Device Manager before you can see devices that are not connected to the computer.

When you finish troubleshooting, close Device Manager, type exit at the command prompt.

Note that when you close the command prompt window, Window clears the devmgr_show_nonpresent_devices=1  variable that you set in step 2 and prevents ghosted devices from being displayed when you click 'show hidden devices'.

This worked very well and I was able to configure my new virtual NIC's with no annoying alerts about duplicate registry profiles for network adapters. 

Hope this helps. 

All information is provided on an AS-IS basis, with no  warranties and confers no rights.

[external-link]: http://support.microsoft.com/kb/315539/en-us
