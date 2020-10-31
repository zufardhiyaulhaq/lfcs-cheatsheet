## Filesystem Features: Attributes, Creating, Checking, Mounting
- check swap areas
```
cat /proc/swaps
```

- disable specific swap in /proc/swaps
```
sudo swapoff /dev/dm-1
```

- enable specific swap
```
sudo swapon /dev/dm-1
```

- TODO: quotas operation
- list size of directory
```
du -h <specific-dir>
```
- list also the file size
```
du -ah <specific-dir>
```
- list with max depth 1
```
du -ah -d 1 /
```

## Excercise

### Managing Swap File
- check current swap
```
root@lfcs-0:~# cat /proc/swaps
Filename				Type		Size	Used	Priority
/dev/dm-1                               partition	1003516	0	-2
```
- create file for swap
```
dd if=/dev/zero of=swapfile bs=1M count=1024
```
- format file with mkswap
```
mkswap swapfile
```
- enable swap
```
swapon swapfile
root@lfcs-0:~# cat /proc/swaps
Filename				Type		Size	Used	Priority
/dev/dm-1                               partition	1003516	0	-2
/root/swapfile                          file		1048572	0	-3
```

### Filesystem Quotas
TODO
