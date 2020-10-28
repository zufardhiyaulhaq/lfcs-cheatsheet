## I/O Scheduling

- check I/O schedulers available for specific disk
```
root@lfcs-0:/home/vagrant# cat /sys/block/sda/queue/scheduler
noop deadline [cfq]
```

- change I/O schedulers (root)
```
root@lfcs-0:/home/vagrant# echo deadline > /sys/block/sda/queue/scheduler
root@lfcs-0:/home/vagrant# cat /sys/block/sda/queue/scheduler
noop [deadline] cfq
```

- check disk SSD or harddisk
```
root@lfcs-0:/home/vagrant# cat /sys/block/sda/queue/rotational
1 # not ssd
0 # ssd
```
