## DPKG
- list all package installed
```
root@lfcs-0:/home/vagrant# dpkg -l
```

- list files installed in packages
```
root@lfcs-0:/home/vagrant# dpkg  -L wget
```

- show what packages own the files
```
root@lfcs-0:/home/vagrant# dpkg -S /etc/init/rpcbind.conf
rpcbind: /etc/init/rpcbind.conf

root@lfcs-0:/home/vagrant# dpkg --search /etc/init/rpcbind.conf
rpcbind: /etc/init/rpcbind.conf
```

- show status of a package
```
vagrant@lfcs-0:~$ dpkg -s sqlite3
dpkg-query: package 'sqlite3' is not installed and no information is available
```

- verify integrity
```
vagrant@lfcs-0:~$ dpkg -V sqlite3
vagrant@lfcs-0:~$ dpkg -V wget
```
