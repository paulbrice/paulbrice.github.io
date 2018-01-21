---
layout: post
title: VMware WhiteBox - Home Lab
date: 2013-03-29 19:15:00 -07:00
categories: ad
---
So after reading several VMware home lab setup articles I decided to build my lab.

I started with ESXi 5.0 update1 but I had issues with it recognizing the mSATA disk.

So after downloading ESXi 5.1 all was good!

I am starting out with a single VMware server using the parts below, running ESXi 5.1.

Note: (Purchased March 2013)

    Shuttle xPC SZ77R5
    Intel Core i7-3770 LGA1155
    Crucial mSATA mPCI-E 32GB
    Mushkin DDR3 PC12800 - 32GB RAM Kit
    2 x 1TB Seagate SATA disks
    1 x LightScribe SATA CD-Rom

Finished Box:

![whitebox](/images/25d21-img_0213.jpg)

All hardware was recognized, even the on-board LAN card and I had ESXi up and running in 30 minutes.

![hardware](/images/screenshot2013-03-29.png)

This box is very quiet and with ESXi 5.1 it was very simple to build.

Note: The motherboard BIOS is the latest "SZ77R000.111"

Questions:
Hi Paul,
do you use the integrated SSD-Caching of the Intel Board with VMware ESX?
Answer:
No, I used the mSATA card as the primary operating system disk. (The BIOS was set in mSATA mode for the slot to use this).

Hope this helps.
All information is provided on an AS-IS basis, with no warranties and confers no rights.
