## Disk partitioning

- show information about partitions
```
blkid /dev/sda1
blkid /dev/sda*
```

- present block device in tree format
```
root@lfcs-0:/home/vagrant# lsblk
NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                      8:0    0   64G  0 disk
└─sda1                   8:1    0   64G  0 part
  ├─vagrant--vg-root   253:0    0   63G  0 lvm  /
  └─vagrant--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
```

- backup partition tables
```
dd if=/dev/sda of=backupmbr bs=512 count=1
```

- restored partition tables
```
dd if=backupmbr of=/dev/sda bs=512 count=1
```

- GPT systems partition tables
```
sgdisk --backup=/tmp/backupmbr /dev/sda
```

- doing something in /dev/sda
```
fdisk /dev/sda
partprobe -s
```

### Naming convention
Device nodes for SCSI and SATA disks follow a simple xxy[z] naming convention, where xx is the device type (usually sd), y is the letter for the drive number (a, b, c, etc.), and z is the partition number

- The first hard disk is /dev/sda
- The second hard disk is /dev/sdb
- /dev/sdb1 is the first partition on the second disk
- /dev/sdc4 is the fourth partition on the third disk.

## Exercise

### Using a file as disk partition image
- create file
```
dd if=/dev/zero of=filedisk bs=1M count=2048
```

- create filesystems
```
mkfs.ext4 filedisk
```

- mount with loop devices
```
mount -o loop filedisk /mnt
```

- unmount
```
unmount /mnt
```

### Partitioning a disk image file
- using fdisk
```
fdisk filedisk

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1):
First sector (2048-4194303, default 2048):
Last sector, +sectors or +size{K,M,G,T,P} (2048-4194303, default 4194303): +256M

Created a new partition 1 of type 'Linux' and of size 256 MiB.

Command (m for help): p
Disk filedisk: 2 GiB, 2147483648 bytes, 4194304 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x2a5adb47

Device     Boot Start    End Sectors  Size Id Type
filedisk1        2048 526335  524288  256M 83 Linux

Command (m for help): w
The partition table has been altered.
Syncing disks.
```

### using iosetup
- get free loop devices
```
root@lfcs-0:/home/vagrant# losetup -f
/dev/loop0
```

- Associate the image file with a loop device
```
losetup /dev/loop0 filedisk -P
losetup -a
```

- check partition (already created)
```
root@lfcs-0:/home/vagrant# fdisk -l /dev/loop0
Disk /dev/loop0: 2 GiB, 2147483648 bytes, 4194304 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x2a5adb47

Device       Boot Start    End Sectors  Size Id Type
/dev/loop0p1       2048 526335  524288  256M 83 Linux
```

- put filesystem
```
mkfs.ext4 /dev/loop0p1
```

- mount
```
mount /dev/loop0p1 /mnt
```

- remove loop devices
```
umount /mnt
losetup -d /dev/loop0
```
