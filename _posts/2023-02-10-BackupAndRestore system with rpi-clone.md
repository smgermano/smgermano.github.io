
## How to Backup and Restore Raspberry Pi OS system

#### rpi-clone (simple mode)
Clone system from SD card to USB-SATA SSD

[rpi-clone](https://github.com/billw2/rpi-clone)

#### rpi-clone (used to backup and boot from different source)
The system is booting from SD-SATA adapter 480 GB

It needs to make changes in fstab, but after that we want to make sure it is booting from SDcard, because if the process fail. We can extract SD card and modify /etc/fstab

Steps:

1. Know where I booting? Try this command
  ```  
  $ mount | egrep "/([[:space:]]|boot)"
  ```
  /dev/sdb2 on / type ext4 (rw,noatime)
  /dev/sdb1 on /boot type vfat (rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,flush,errors=remount-ro)

  (booting from sdb1, SATA-USB adapter SD disk )

  Another method will be use "rpi-clone", 
  ``rpi-clone`` and for example mmcblk0
  
  a) get this message if boot from mmcblk0 ``Destination disk sdb is the booted disk.  Cannot clone!``
  
  b) get this message instead, ``Ok to proceed with the clone?  (yes/no): `` (choose "no" if you don't whant to clone)

2. Clone sdb -> mmcblk0 and change boot from SDcard
````
$ sudo rpi-clone mmcblk0
````
3. Modificar el archivo fstab


4. #### Pendiente

