---
layout: post
title: How to Create a RAID 5 Storage Array with ‘mdadm’ on Ubuntu 
tags: [work]
---

### 1. Creating the New RAID 5 Array using the ‘mdadm’ command.
Before we start any thing we will check the existing the disk attached to the machine. 
Below is the command to list the available disks.

```
$ lsblk –o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT
Output
NAME    SIZE FSTYPE              TYPE MOUNTPOINT
sda       20G                    disk
sdb       20G                    disk
sdc       20G linux_raid_member  disk
vda       20G                    disk
├─vda1    20G ext4               part /
└─vda15   1M                     part
```

As we can see in the above output, we have 3 disks without any filesystem with 20GB and the devices are named as 
/dev/sda, /dev/sdb and /dev/sdc for this machine or session.

For creating the RAID 5 array, we will use the mdadm – to create the command with the device name, 
we want to create and the raid level with the no of devices attaching to the RAID.

```
$ sudo mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/sda /dev/sdb /dev/sdc
```

The mdadm tool will start the creation of an array it will take some time to complete the configuration, 
we can monitor the progress using the below command

```
$ cat /proc/mdstat
Output
Personalities : [raid1] [linear] [multipath] [raid0] [raid6] [raid5] [raid4] [raid10]
md0 : active raid5 sdc[3] sdb[1] sda[0]
   24792064 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/2] [UU_]
   [===>.................] recovery = 15.6% (16362536/24792064) finish=7.3min speed=200808K/sec
unused devices: <none>
```

In the above output we can see the /dev/md0 device is being created with RAID 5 using the /dev/sda, /dev/sdb and /dev/sdc storage devices, 
this will also show the progress on the raid device.

### 2. Creating and Mounting the Filesystem.

Before we mount the Array disk, we need to create a filesystem on the array disk which we created using the above steps.

We will create a filesystem on the array

```
$ sudo mkfs.ext4 –F /dev/md0
```

We will now create a mount point and attaché the new RAID disk created in the above steps.

```
$ sudo mkdir –p /mnt/raiddisk1
$ sudo mount /dev/md0 /mnt/raiddisk1
```

### 3. Verifying the New Mount Point or RAID Disk.

```
$ df –h –x devtmpfs –x tmpfs
Output
Filesystem    Size    Used    Avail    Use%    Mounted on
/dev/vda1      20G    1.1G    18G       6%       /
/dev/md0       40G    60M     39G       1%       /mnt/raiddisk1
```

As we can see, the new filesystem is mounted and accessible.

Now we can scan the active array and append the file with the below command

```
$ sudo mdadm –details –scan | sudo tee –a /etc/mdadm/mdadm.conf
```

We needed to update the ‘initramfs’ file so that the RADI array will be available when 
the machine get started with the boot process.

```
$ sudo update-initramfs -u
```

Adding the RAID array to mount automatically at the boot time.

Add the below line to the /etc/fstab.

```
/dev/md0    /mnt/raiddisk1    ext4    defaults,nofail,discard 0 0
```

> In the above setup and configuration we have configured a RAID 5 level array using three disks and 
mounted the disk at the boot time so that when ever 
we restart the server the raid disk will be loaded.
