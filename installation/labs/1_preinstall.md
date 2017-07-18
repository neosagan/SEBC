## 1. Configure swappiness
`sudo vi /etc/sysctl.conf`
Add the line `vm.swappiness=1`
`cat /proc/sys/vm/swappiness`

## 2. Check mount type of volumes
`mount -t ext3`
Output: `/dev/xvdb on /mnt type ext3 (rw,relatime,data=ordered)`

## 3. Check reserve space setting
`sudo tune2fs -l /dev/xvdb`
Output: `Reserved block count:     491417`

## 4. Disable Transparent Huge Page
`echo 'never' > /sys/kernel/mm/transparent_hugepage/defrag`

## 5. Check network interface
`ifconfig`
Output: ```eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
        inet 172.31.40.46  netmask 255.255.240.0  broadcast 172.31.47.255
        inet6 fe80::4a9:eff:fec4:7518  prefixlen 64  scopeid 0x20<link>
        ether 06:a9:0e:c4:75:18  txqueuelen 1000  (Ethernet)
        RX packets 1941  bytes 144967 (141.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1591  bytes 177242 (173.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        ```

## 6. Check reverse and forward lookups
`host 172.31.40.46`
Output: `46.40.31.172.in-addr.arpa domain name pointer ip-172-31-40-46.us-west-2.compute.internal.`

`nslookup 172.31.40.46`

Output: ```Server:         172.31.0.2
        Address:        172.31.0.2#53
        Non-authoritative answer:
        46.40.31.172.in-addr.arpa       name = ip-172-31-40-46.us-west-2.compute.internal.```

## 7. Install and start nscd and ntp services 
`sudo yum install nscd`
`sudo systemctl start nscd`
`systemctl status nscd`

Output: ```nscd.service - Name Service Cache Daemon
   Loaded: loaded (/usr/lib/systemd/system/nscd.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2017-07-17 14:48:55 EDT; 4s ago
   ```
`sudo yum install ntp`
`sudo systemctl start ntp`
`systemctl status ntp`

Output: ```ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2017-07-17 14:49:51 EDT; 5s ago
   ```

## 8. Install Master MariaDB
`touch /etc/yum.repos.d/MariaDB.repo`

Add to the repo file
```# MariaDB 10.0 RedHat repository list - created 2017-07-17 18:52 UTC
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.0/rhel7-ppc64le
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```
`sudo yum install mariadb-server mariadb-client`

