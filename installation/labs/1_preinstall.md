<code>sudo vi /etc/sysctl.conf
vm.swappiness=1
cat /proc/sys/vm/swappiness</code>

<code>mount -t ext3
/dev/xvdb on /mnt type ext3 (rw,relatime,data=ordered)</code>

<code>sudo tune2fs -l /dev/xvdb</code>
Reserved block count:     491417

<code>
echo 'never' > /sys/kernel/mm/transparent_hugepage/defrag
</code>

<code>
ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
        inet 172.31.40.46  netmask 255.255.240.0  broadcast 172.31.47.255
        inet6 fe80::4a9:eff:fec4:7518  prefixlen 64  scopeid 0x20<link>
        ether 06:a9:0e:c4:75:18  txqueuelen 1000  (Ethernet)
        RX packets 1941  bytes 144967 (141.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1591  bytes 177242 (173.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
</code>

<code>
host 172.31.40.46
46.40.31.172.in-addr.arpa domain name pointer ip-172-31-40-46.us-west-2.compute.internal.
</code>

<code>
nslookup 172.31.40.46

Server:         172.31.0.2
Address:        172.31.0.2#53
Non-authoritative answer:
46.40.31.172.in-addr.arpa       name = ip-172-31-40-46.us-west-2.compute.internal.
</code>

<code>
sudo yum install nscd
sudo systemctl start nscd
systemctl status nscd

nscd.service - Name Service Cache Daemon
   Loaded: loaded (/usr/lib/systemd/system/nscd.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2017-07-17 14:48:55 EDT; 4s ago
</code>

<code>
sudo yum install ntp
sudo systemctl start ntp
systemctl status ntp

ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2017-07-17 14:49:51 EDT; 5s ago
</code>

<code>
touch /etc/yum.repos.d/MariaDB.repo

# MariaDB 10.0 RedHat repository list - created 2017-07-17 18:52 UTC
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.0/rhel7-ppc64le
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1

sudo yum install mariadb-server mariadb-client
</code>
