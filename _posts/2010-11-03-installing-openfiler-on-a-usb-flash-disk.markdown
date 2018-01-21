---
layout: post
title: Installing OpenFiler on a USB Flash Disk
date: 2010-11-03 22:58:00 -07:00
categories: openfiler
---
A few days ago I was searching the internet for good articles on installing OpenFiler onto a USB Flash disk for my home NAS storage and I found [this][external-link] article written by "Rynardt Spies" on his site http://www.virtualvcp.com/home and apart from a few issues it was excellent!

I had no issues with steps 1 to 15, the remaining steps have been updated with my experiences.

Step 16:
Now we need to edit the init file under /tmp/tmp-initrd/. I like to use vi as my text editor. So, here we go. 

- Type "cd /tmp/tmp-initrd/"  
- Type "vi init"  
- Press "i"  to enter Insert Mode  
- Using arrow keys find the line with insmod /lib/sd_mod.ko
- Enter the following lines after it
 - insmod /lib/rs_mod.ko
 - insmod /lib/ehci-hcd.ko
 - insmod /lib/uhci-hcd.ko
 - sleep 5 echo “Loading USB Storage Drivers”
 - insmod /lib/usb-storage.ko
 - sleep 10
- Save the file and quit vi:  
 - Press "ESCAPE"  to exit insert mode.
 - Type ":wq"  to save and exit.  

Step 17:

Now we have configured the init script to load some modules when the  kernel boots, however, we now need to copy the actual module files to  /lib/ directory.

- Browse to drivers directory, Type cd /lib/modules/  

**Note the next directory will depend on your OpenFiler build version**  

- Browse to the next sub directory
- Type  cd /%Your-Version%/kernel/drivers  
- Copy new modules  
 - Type "cp usb/storage/usb-storage.ko /tmp/tmp-initrd/lib/"
 - Type "cp usb/host/ehci-hcd.ko /tmp/tmp-initrd/lib/cp usb/host/uhci-hcd.ko /tmp/tmp-initrd/lib/"
 - Type "cp scsi/sr_mod.ko /tmp/tmp-initrd/lib/"

Step 18:

Now we need to pack it all in a .img file and place it in  /boot/. We then need to tell grub (the boot loader) where to find it.

- Type "cd /tmp/tmp-initrd"
- Type "find . |cpio –c –o | gzip -9 &gt; /boot/usb-initrd.img"
- Type "cd /boot/grub"
- Type "vi grub.conf"  
- Press "i"  to enter Insert Mode, find the line that contains the old initrd file and change the file name to usb-initrd.img  
- Press "ESCAPE"  to exit Insert Mode and type ":wq"  to save the grub.conf file and exit.  

To  reboot, type "exit" twice. The server should reboot and should now be  able to mount /dev/sdb3when it boots from the USB stick.  

Hope this helps. 

All information is provided on an AS-IS basis, with no warranties and confers no rights.

[external-link]: http://www.virtualvcp.com/linux-technical-guides/122-building-an-iscsi-openfiler-san-on-a-usb-stick
