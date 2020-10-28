## APT
- Search repository for package
```
vagrant@lfcs-0:~$ apt-cache search vagrant
```
- display basic information
```
vagrant@lfcs-0:~$ apt-cache show vagrant
```
- display detail information
```
vagrant@lfcs-0:~$ apt-cache showpkg vagrant
```
- list app depedency
```
vagrant@lfcs-0:~$ apt-cache depends vagrant
```
- search file in all package (get from repository)
```
vagrant@lfcs-0:~$ apt-file search bind9.conf
bind9: /usr/lib/tmpfiles.d/bind9.conf
dms-core: /etc/dms/server-config-templates/bind9.conf
```
- list all file in package
```
vagrant@lfcs-0:~$ apt-file list bind9
bind9: /etc/apparmor.d/usr.sbin.named
bind9: /etc/bind/bind.keys
bind9: /etc/bind/db.0
bind9: /etc/bind/db.127
bind9: /etc/bind/db.255
...
```
