## Linux filesystems and the VFS

- creating hard link
```
ln <source> <hard-link>
```

- create symbolic link
```
ln -s <source> <symbolic-link>
```

- list mounted filesystems
```
cat /proc/filesystems
```

## Excercise

### Mount XFS filesystems
- create container as filesystem
```
dd if=/dev/zero of=junk bs=10M
```

- put filesystems
```
mkfs.xfs junk
```

- mount junk on mnt
```
mount junk /mnt
```

- check mount
```
df -h
```
