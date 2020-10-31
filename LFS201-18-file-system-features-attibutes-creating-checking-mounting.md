## Filesystem Features: Attributes, Creating, Checking, Mounting
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
- mount NFS every reboot
```
vi /etc/fstab
network.com:/shared_dir /mnt/shared_dir nfs defaults 0 0
```
- mount device every reboot
```
vi /etc/fstab
/dev/sda2 /mnt/data ext4 defaults 1 2
```

## Excercise

### Working with file attributes
-  With your normal user account use touch to create an empty file named /tmp/appendit
```
touch /tmp/appendit
```
-  Use cat to copy the contents of /etc/hosts to /tmp/appendit
```
cat /etc/hosts > /tmp/appendit
```
-  Compare the contents of /tmp/appendit with /etc/hosts; there should not be any differences.
```
diff /etc/hosts /tmp/appendit
```
-  Try to add the append-only attribute to /tmp/appendit by using chattr. You should see an error here.
```
chattr +a /tmp/appendit
```
-  As root, retry adding the append-only attribute; this time it should work.  Look at the file’s extended attributes by using lsattr
```
chattr +a /tmp/appendit
lsattr /tmp/appendit
```
-  As a normal user, try and use cat to copy over the contents of /etc/passwd to /tmp/appendit.  You should get an error.
```
cat /etc/passwd > /tmp/appendit
```
because the attribute is append only
-  Try the same thing again as root. You should also get an error. Why?
```
cat /etc/passwd > /tmp/appendit
```
because the attribute is append only
-  As the normal user, again use the append redirection operator (>>) and try appending the /etc/passwd file to /tmp/appendit. This should work. Examine the resulting file to confirm.
```
cat /etc/passwd >> /tmp/appendit
```
-  As root, set the immutable attribute on/tmp/appendit, and look at the extended attributes again.
```
chattr +i /tmp/appendit
lsattr /tmp/appendit
```
-  Try appending output to /tmp/appendit, try renaming the file, creating a hard link to the file, and deleting the file as both the normal user and as root.
```
vagrant@lfcs-0:~$ sudo rm -rf /tmp/appendit
rm: cannot remove '/tmp/appendit': Operation not permitted
```

### Mounting options
- create file for disk
```
dd if=/dev/zero of=datadisk bs=1M count=1024
```

- get free loop devices
```
losetup -f
/dev/loop1
```

- associate loop with device
```
losetup /dev/loop1 datadisk -P
```

- create partition and create filesystem
```
fdisk /dev/loop1
Device       Boot Start    End Sectors  Size Id Type
/dev/loop1p1       2048 206847  204800  100M 83 Linux

partprobe -s
mkfs.ext4 /dev/loop1p1
```

- create sub directory and mount
```
mkdir /mnt/tempdir
mount /dev/loop1p1 /mnt/tempdir
```

- try to create some file
```
echo "lol" > /mnt/tempdir/lol
cat /mnt/tempdir/lol

cp /bin/ls /mnt/tempdir/
/mnt/tempdir/ls
```

- remount as read-only
```
mount -o remount,ro /mnt/tempdir
touch /mnt/tempdir/data
```

- add into fstab
```
echo "/home/vagrant/datadisk /mnt/tempdir loop 1 2" >> /etc/fstab
```
