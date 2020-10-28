## Filessytem Features
- display attibutes for a file
```
lsattr <filename>
```
- check for error for unmounted filesystem
```
fsck <device-file>
```
- check for error in root filesystem
```
touch /forcefsck
reboot
```
- mounting
```
mount /dev/sda2 /mnt
mount LABEL=data /mnt
mount UUID=26d58ee2-9d20-4dc7-b6ab-aa87c3cfb69a /mnt
```
- remount with read only attribute
```
mount -o remount,ro /mnt
```
- list all mounted
```
mount
```
- unmount
```
umount /mnt
umount /dev/sda2
```
- find user who access /root/
```
fuser -c /root
```
- mount NFS
```
mount -t nfs network.com:/shared_dir /mnt/shared_dir
```
- mount every reboot
```
vi /etc/fstab
network.com:/shared_dir /mnt/shared_dir nfs defaults 0 0
```
