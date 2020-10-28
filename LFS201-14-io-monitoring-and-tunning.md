## I/O Monitoring and Tunning

### iostat
- show I/O like transaction per second, blocks read and written
```
iostat [OPTIONS] [devices] [interval] [count]
```
- show with device name
```
iostat -N
```
- show details
```
iostat -k
```

### iotop
show current I/O and updated periodicly
```
iotop
```

### ionice
set both the I/O scheduling class and priority for a given process. can both set running proccess with `-p` or running a new command
```
ionice [-c class] [-n priority] [-p pid ] [COMMAND [ARGS] ]
```

class value:
- 0, none
- 1, real time
- 2, best effort
- 3, idle

only real time and best effort can have priority set `-n` range from 0 to 7, with 0 is higest priority.

