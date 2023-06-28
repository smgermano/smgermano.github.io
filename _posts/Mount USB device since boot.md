## Mount USB device since boot

Need to mount 3 devices since boot

1. Create mount destinations:
```
mkdir /mnt/PNY128
mkdir /mnt/Glacier_1GB
mkdir /mnt/rpi-ssd
```
2. ```
$ blkid
/dev/mmcblk0p1: LABEL_FATBOOT="boot" LABEL="boot" UUID="37E2-62C3" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="6ee70e10-01"
/dev/mmcblk0p2: LABEL="rootfs" UUID="6a932c1f-7335-42d9-9351-1b1b2ca538d4" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="6ee70e10-02"
/dev/sda1: LABEL="Glacier_1TB" UUID="0637114b-6440-4128-bcf2-65a7a9f965c9" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="Glacier_1TB" PARTUUID="9ebce907-cf66-4f9c-b48f-e8da11a709e2"
/dev/sdb1: UUID="97B5-0184" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="097569f8-01"
/dev/sdb2: LABEL="rpi-OS" UUID="57072a56-bc5e-4ca5-937e-97eddb55e1a6" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="097569f8-02"
/dev/sdc1: LABEL="PNY128" BLOCK_SIZE="512" UUID="A8087C0F087BDB30" TYPE="ntfs" PARTUUID="0003f4bf-01"
```
or 
```
$ ls -l /dev/disk/by-uuid/*
lrwxrwxrwx 1 root root 10 Jun 22 19:27 /dev/disk/by-uuid/0637114b-6440-4128-bcf2-65a7a9f965c9 -> ../../sda1
lrwxrwxrwx 1 root root 15 Jun 22 19:27 /dev/disk/by-uuid/37E2-62C3 -> ../../mmcblk0p1
lrwxrwxrwx 1 root root 10 Jun 22 19:27 /dev/disk/by-uuid/57072a56-bc5e-4ca5-937e-97eddb55e1a6 -> ../../sdb2
lrwxrwxrwx 1 root root 15 Jun 22 19:27 /dev/disk/by-uuid/6a932c1f-7335-42d9-9351-1b1b2ca538d4 -> ../../mmcblk0p2
lrwxrwxrwx 1 root root 10 Jun 22 19:27 /dev/disk/by-uuid/97B5-0184 -> ../../sdb1
lrwxrwxrwx 1 root root 10 Jun 22 19:27 /dev/disk/by-uuid/A8087C0F087BDB30 -> ../../sdc1
```
To more detail about disk 
```$ sudo fdisk -l```

In the case of ntfs partitions must install ntfs-3g
```$ sudo apt install ntfs-3g```

3. Edit fstab 
```
$ cp sudo /etc/fstab /etc/fstab.old
$ sudo vim /etc/fstab 
```
Add these lines to fstab :
UUID=0637114b-6440-4128-bcf2-65a7a9f965c9 /mnt/Glacier_1TB ext4 defaults,auto,users,rw,nofail,noatime 0 0
UUID=A8087C0F087BDB30 /mnt/PNY128 ntfs defaults,auto,users,rw,nofail,noatime 0 0
UUID=57072a56-bc5e-4ca5-937e-97eddb55e1a6 /mnt/rpi-ssd ext4 defaults,auto,users,rw,nofail,noatime 0 0
