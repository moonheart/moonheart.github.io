---
title: esxi 中的 CentOS lvm扩容磁盘
date: 2020-01-13T21:21:00+08:00
draft: false
---

1. 首先在 vCenter 控制面板中增加磁盘容量并重启系统

``` shell
root@server ~]# fdisk /dev/sda
Command (m for help): p

Disk /dev/sda: 171.8 GB, 171798691840 bytes, 335544320 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000dbca9

Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200   167772159    82836480   83  Linux

Command (m for help): d
Partition number (1,2, default 2): 2
Partition 2 is deleted

Command (m for help): n
Partition type:
p   primary (1 primary, 0 extended, 3 free)
e   extended
Select (default p): p
Partition number (2-4, default 2):
First sector (2099200-335544319, default 2099200):
Using default value 2099200
Last sector, +sectors or +size{K,M,G} (2099200-335544319, default 335544319):
Using default value 335544319
Partition 2 of type Linux and of size 159 GiB is set

Command (m for help): p

Disk /dev/sda: 171.8 GB, 171798691840 bytes, 335544320 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000dbca9

Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200   335544319   166722560   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or   resource busy
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.
```

2. 使用 fdisk 扩容分区
``` shell
root@server /]# pvresize /dev/sda2
Physical volume "/dev/sda2" changed
1 physical volume(s) resized or updated / 0 physical volume(s) not resized
```

3. 使用 pvresize
``` shell
root@server /]# pvresize /dev/sda2
Physical volume "/dev/sda2" changed
1 physical volume(s) resized or updated / 0 physical volume(s) not resized
```

4. 使用 lvextend 扩容扩容 lvm

``` shell
root@server /]# lvextend -l +100%FREE /dev/mapper/centos-root
Size of logical volume centos/root changed from 75.12 GiB (19231 extents) to 155.12 GiB (39711 extents).
Logical volume centos/root successfully resized.
```

5. 使用 xfs_growfs

``` shell
root@server /]# xfs_growfs /dev/centos/root
meta-data=/dev/mapper/centos-root isize=512    agcount=9, agsize=2301440 blks
=                       sectsz=512   attr=2, projid32bit=1
=                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=19692544, imaxpct=25
=                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=4495, version=2
=                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 19692544 to 40664064
```