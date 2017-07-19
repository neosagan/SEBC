### Challenge Setup

* I am using AWS cloud provider with m3.xlarge instances

* The configured IP addresses and DNS:
```
172.31.34.86    ip-172-31-34-86.us-west-2.compute.internal
172.31.44.49    ip-172-31-44-49.us-west-2.compute.internal
172.31.39.144   ip-172-31-39-144.us-west-2.compute.internal
172.31.42.241   ip-172-31-42-241.us-west-2.compute.internal
172.31.34.127   ip-172-31-34-127.us-west-2.compute.internal
```

* The linux release
```
 Operating System: CentOS Linux 7 (Core)
 CPE OS Name: cpe:/o:centos:centos:7
 Kernel: Linux 3.10.0-514.16.1.el7.x86_64
 Architecture: x86-64
 ```     
 
 * Filesystems capacity
 ```
 Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       30G  1.2G   29G   4% /
devtmpfs        7.3G     0  7.3G   0% /dev
tmpfs           7.2G     0  7.2G   0% /dev/shm
tmpfs           7.2G   17M  7.2G   1% /run
tmpfs           7.2G     0  7.2G   0% /sys/fs/cgroup
/dev/xvdc        37G   49M   35G   1% /data0
/dev/xvdb        37G   49M   35G   1% /mnt
tmpfs           1.5G     0  1.5G   0% /run/user/0
tmpfs           1.5G     0  1.5G   0% /run/user/1000
 ```
 
* Output for `yum repolist enabled`
```
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.web-ster.com
 * extras: mirror.tocici.com
 * updates: mirror.chpc.utah.edu
repo id                             repo name                             status
base/7/x86_64                       CentOS-7 - Base                       9,363
extras/7/x86_64                     CentOS-7 - Extras                       447
updates/7/x86_64                    CentOS-7 - Updates                    2,100
repolist: 11,910
```

### Create linux users and groups
```
sudo groupadd comets
sudo groupadd planets
sudo useradd -u 2800 -g comets haley
sudo passwd haley
sudo useradd -u 2900 -g planets saturn
sudo passwd saturn
```

### Output `/etc/passwd`
```
haley:x:2800:1001::/home/haley:/bin/bash
saturn:x:2900:1002::/home/saturn:/bin/bash
```

### Output `/etc/groups`
```
comets:x:1001:
planets:x:1002:
```
