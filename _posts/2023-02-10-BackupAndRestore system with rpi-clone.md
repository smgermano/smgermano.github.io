rpi-clone (simple mode)
Clone system from SD card to USB-SATA SSD
https://github.com/billw2/rpi-clone

rpi-clone (used to backup and boot from different source)
The system is booting from SD-SATA adapter 480 GB

It needs to make changes in fstab, but after that we want to make sure it is booting from SDcard, because if the process fail. We can extract SD card and modify /etc/fstab

Steps:

1. Show where i'm booting

mount | egrep "/([[:space:]]|boot)"

/dev/sdb2 on / type ext4 (rw,noatime)
/dev/sdb1 on /boot type vfat (rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,flush,errors=remount-ro)

(booting from sdb1, SATA-USB adapter SD disk )

Wait several time

2. Clone sdb -> mmcblk0 and change boot from SDcard

  sudo rpi-clone mmcblk0

Booted disk: sdb 480.1GB               	Destination disk: mmcblk0 127.9GB
---------------------------------------------------------------------------
Part  	Size	FS 	Label       	Part   Size	FS 	Label   
1 /boot   256.0M  fat32  --          	1  	256.0M  fat32  -- 	 
2 root	446.9G  ext4   rpi-OS      	2  	118.9G  ext4   rootfs  
---------------------------------------------------------------------------
== SYNC sdb file systems to mmcblk0 ==
/boot             	(30.0M used)   : SYNC to mmcblk0p1 (256.0M size)
/                 	(30.1G used)   : SYNC to mmcblk0p2 (118.9G size)
---------------------------------------------------------------------------
Run setup script   	: no.
Verbose mode       	: no.
-----------------------:

Ok to proceed with the clone?  (yes/no):     

Syncing file systems (can take a long time)
Syncing mounted partitions:
  Mounting /dev/mmcblk0p2 on /mnt/clone
  => rsync // /mnt/clone with-root-excludes ...
  Mounting /dev/mmcblk0p1 on /mnt/clone/boot
  => rsync /boot/ /mnt/clone/boot  ...

Editing /mnt/clone/boot/cmdline.txt PARTUUID to use 6ee70e10
Editing /mnt/clone/etc/fstab PARTUUID to use 6ee70e10
===============================
Done with clone to /dev/mmcblk0
   Start - 17:35:03	End - 17:42:01	Elapsed Time - 6:58

Cloned partitions are mounted on /mnt/clone for inspection or customizing.


 Make changes of fstab (than can may editables if fail by extracting SD card)
 If changes works, follow step 3

Pendiente:
========
3. Clone mmcblk0 -> sdb and change boot from SSD sdb

4. Keep backup :

  -> USB boot clone back to SD card slot that preserves SD card to USB boot setup:

    $ rpi-clone -l mmcblk0

	-l  	- leave SD card to USB boot alone when cloning to SD card mmcblk0
            	from a USB boot.  This preserves a SD card to USB boot setup
            	by leaving the SD card cmdline.txt using the USB root.    When
            	cloning to USB from SD card this option sets up the SD card
            	cmdline.txt to boot to the USB disk.
