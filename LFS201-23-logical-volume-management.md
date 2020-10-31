## Logical Volume Management (LVM)
- displying physical volumes
```
pvdisplay
```
- displying volume groups
```
vgdisplay
```
- displying logical volumes
```
lvdisplay 
```
- create physical volumes
format into `8e`
```
pvcreate /dev/sdb1
pvcreate /dev/name
```
- create volume group
```
vgcreate my-vg-name /dev/sdb1
```
- extend volume group
```
vgextend my-vg-name /dev/name
```
- creating logical volume
```
lvcreate -L 20G -n my-lv-name my-vg-name
```
- using logical volume
```
mkfs.ext4 /dev/my-vg-name/my-lv-name
mkdir /mnt/lv-mount
mount /dev/my-vg-name/my-lv-name /mnt/lv-mount

vi /etc/fstab
/dev/my-vg-name/my-lv-name /mnt/lv-mount ext4 default 1 2
```
- resizing logical volume
```
lvresize --resizefs -L +20G /dev/my-vg-name/my-lv-name
```
- create snapshot of logical volume
```
lvcreate -L 1G -s -n my-lv-name-snapshot /dev/my-vg-name/my-lv-name
```
- restore snapshot
```
lvconvert --merge /dev/my-vg-name/my-lv-name
```
- remove snapshot
```
lvremove /dev/my-vg-name/my-lv-name
```

## Excercise
### Create LVM from loop devices
- create two disk files
```
dd if=/dev/zero of=/root/lvm-disk-0 count=2048 bs=1M
dd if=/dev/zero of=/root/lvm-disk-1 count=2048 bs=1M
```
- adding loop
```
losetup -f
losetup /dev/loop2 /root/lvm-disk-0 -P

losetup -f
losetup /dev/loop3 /root/lvm-disk-1 -P
```
- create partition
```
fdisk /dev/loop2
Device       Boot   Start     End Sectors  Size Id Type
/dev/loop2p1         2048 2099199 2097152    1G 8e Linux LVM
/dev/loop2p2      2099200 4194303 2095104 1023M 8e Linux LVM

fdisk /dev/loop3
Device       Boot   Start     End Sectors  Size Id Type
/dev/loop3p1         2048 2099199 2097152    1G 8e Linux LVM
/dev/loop3p2      2099200 4194303 2095104 1023M 8e Linux LVM

partprobe -s
```
- create physical volume
```
root@lfcs-0:/home/vagrant# pvcreate /dev/loop2p1
  Physical volume "/dev/loop2p1" successfully created.
root@lfcs-0:/home/vagrant# pvcreate /dev/loop2p2
  Physical volume "/dev/loop2p2" successfully created.
root@lfcs-0:/home/vagrant# pvcreate /dev/loop3p1
  Physical volume "/dev/loop3p1" successfully created.
root@lfcs-0:/home/vagrant# pvcreate /dev/loop3p2
  Physical volume "/dev/loop3p2" successfully created.

root@lfcs-0:/home/vagrant# pvs
  PV           VG         Fmt  Attr PSize    PFree
  /dev/loop2p1            lvm2 ---     1.00g    1.00g
  /dev/loop2p2            lvm2 ---  1023.00m 1023.00m
  /dev/loop3p1            lvm2 ---     1.00g    1.00g
  /dev/loop3p2            lvm2 ---  1023.00m 1023.00m
  /dev/sda1    vagrant-vg lvm2 a--   <64.00g       0
```
- create volume group
```
vgcreate my-vg-name /dev/loop2p1
vgextend my-vg-name /dev/loop2p2
vgextend my-vg-name /dev/loop3p1
vgextend my-vg-name /dev/loop3p2

root@lfcs-0:/home/vagrant# vgdisplay
  --- Volume group ---
  VG Name               my-vg-name
  System ID
  Format                lvm2
  Metadata Areas        4
  Metadata Sequence No  4
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                4
  Act PV                4
  VG Size               3.98 GiB
  PE Size               4.00 MiB
  Total PE              1020
  Alloc PE / Size       0 / 0
  Free  PE / Size       1020 / 3.98 GiB
  VG UUID               e0aMWC-jhrT-V2tF-3stk-0Q5A-K2rg-fSw3pr
```
- create logical volume
```
root@lfcs-0:/home/vagrant# lvcreate -L 1G -n my-lv-name my-vg-name
  Logical volume "my-lv-name" created.
```
- mount
```
mkfs.ext4 /dev/my-vg-name/my-lv-name
mkdir /mnt/my-lv-mount
mount /dev/my-vg-name/my-lv-name /mnt/my-lv-mount

df -h
/dev/mapper/my--vg--name-my--lv--name  976M  2.6M  907M   1% /mnt/my-lv-mount
```
- add another size
```
lvresize -L +300M -r /dev/my-vg-name/my-lv-name
or 
lvextend -L +300M -r /dev/my-vg-name/my-lv-name
```
