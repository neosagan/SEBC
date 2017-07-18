### 1. Configure swappiness
    sudo vi /etc/sysctl.conf
* Add the line `vm.swappiness=1`
* Reboot and check the change with `cat /proc/sys/vm/swappiness`

### 2. Check mount type of volumes
    mount -t ext3
**Output:** `/dev/xvdb on /mnt type ext3 (rw,relatime,data=ordered)`

### 3. Check reserve space setting
    sudo tune2fs -l /dev/xvdb
**Output:** `Reserved block count: 491417`

### 4. Disable Transparent Huge Page
    echo never > /sys/kernel/mm/transparent_hugepage/defrag
    echo never > /sys/kernel/mm/transparent_hugepage/enabled

### 5. Check network interface
    ifconfig
**Output:**

    eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
    inet 172.31.40.46  netmask 255.255.240.0  broadcast 172.31.47.255
    inet6 fe80::4a9:eff:fec4:7518  prefixlen 64  scopeid 0x20<link>
    ether 06:a9:0e:c4:75:18  txqueuelen 1000  (Ethernet)
    RX packets 1941  bytes 144967 (141.5 KiB)
    RX errors 0  dropped 0  overruns 0  frame 0
    TX packets 1591  bytes 177242 (173.0 KiB)
    TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
### 6. Check reverse and forward lookups
    host 172.31.40.46
**Output:** `46.40.31.172.in-addr.arpa domain name pointer ip-172-31-40-46.us-west-2.compute.internal.`

    nslookup 172.31.40.46
**Output:**

    Server: 172.31.0.2
    Address: 172.31.0.253
    Non-authoritative answer: 46.40.31.172.in-addr.arpa       
    name = ip-172-31-40-46.us-west-2.compute.internal.

### 7. Install and start nscd and ntp services 
    sudo yum install nscd
    sudo systemctl start nscd
    systemctl status nscd`
**Output:**

    nscd.service - Name Service Cache Daemon
    Loaded: loaded (/usr/lib/systemd/system/nscd.service; disabled; vendor preset: disabled)
    Active: active (running) since Mon 2017-07-17 14:48:55 EDT; 4s ago
   
    sudo yum install ntp
    sudo systemctl start ntp
    systemctl status ntp

**Output:** 
    
    ntpd.service - Network Time Service
    Loaded: loaded (/usr/lib/systemd/system/ntpd.service; disabled; vendor preset: disabled)
    Active: active (running) since Mon 2017-07-17 14:49:51 EDT; 5s ago
   
### 8. Install Master MariaDB
    touch /etc/yum.repos.d/MariaDB.repo

* Add to the repo file
```
# MariaDB 10.0 RedHat repository list - created 2017-07-17 18:52 UTC
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.0/rhel7-ppc64le
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```
* Install the server and client `sudo yum install mariadb-server mariadb-client`

### 9. Create users and databases

```
create database scm DEFAULT CHARACTER SET utf8;
create user 'scm'@'localhost' IDENTIFIED BY 'scm';
grant all on scm.* TO 'scm'@'%' IDENTIFIED BY 'scm';

create database amon DEFAULT CHARACTER SET utf8;
create user 'amon'@'localhost' IDENTIFIED BY 'amon';
grant all on amon.* TO 'amon'@'%' IDENTIFIED BY 'amon';

create database rman DEFAULT CHARACTER SET utf8;
create user 'rman'@'localhost' IDENTIFIED BY 'rman';
grant all on rman.* TO 'rman'@'%' IDENTIFIED BY 'rman';

create database metastore DEFAULT CHARACTER SET utf8;
create user 'metastore'@'localhost' IDENTIFIED BY 'metastore';
grant all on metastore.* TO 'metastore'@'%' IDENTIFIED BY 'metastore';

create database sentry DEFAULT CHARACTER SET utf8;
create user 'sentry'@'localhost' IDENTIFIED BY 'sentry';
grant all on sentry.* TO 'sentry'@'%' IDENTIFIED BY 'sentry';

create database nav DEFAULT CHARACTER SET utf8;
create user 'nav'@'localhost' IDENTIFIED BY 'nav';
grant all on nav.* TO 'nav'@'%' IDENTIFIED BY 'nav';

create database navms DEFAULT CHARACTER SET utf8;
create user 'navms'@'localhost' IDENTIFIED BY 'navms';
grant all on navms.* TO 'navms'@'%' IDENTIFIED BY 'navms';

create database hue DEFAULT CHARACTER SET utf8;
create user 'hue'@'localhost' IDENTIFIED BY 'hue';
grant all on hue.* to 'hue'@'localhost' identified by 'hue';

create database oozie;
create user 'oozie'@'localhost' IDENTIFIED BY 'oozie';
grant all privileges on oozie.* to 'oozie'@'localhost' identified by 'oozie';
grant all privileges on oozie.* to 'oozie'@'%' identified by 'oozie';

create database sqoop;
create user 'sqoop'@'localhost' IDENTIFIED BY 'sqoop';
grant all privileges on sqoop.* to 'sqoop'@'localhost' identified by 'sqoop';
grant all privileges on sqoop.* to 'sqoop'@'%' identified by 'sqoop';
```

**Output:** 

```
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| amon               |
| hue                |
| metastore          |
| mysql              |
| nav                |
| navms              |
| oozie              |
| performance_schema |
| rman               |
| scm                |
| sentry             |
| sqoop              |
+--------------------+
13 rows in set (0.01 sec)
```

### 11. Install Java 7
    sudo yum install java-1.7.0-openjdk-devel
    java -version

**Output:**

    java version "1.7.0_141"
    OpenJDK Runtime Environment (rhel-2.6.10.1.el7_3-x86_64 u141-b02)
    OpenJDK 64-Bit Server VM (build 24.141-b02, mixed mode)

### 10. Download and install the jdbc driver
    tar vxf mysql-connector-java-5.1.42.tar.gz
    cd mysql-connector-java-5-1-42
    mv mysql-connector-java-5-1-42.jar mysql-connector-java.jar
    sudo cp mysql-connector-java.jar /usr/share/java
    
### 11. Add the repo and install Cloudera Manager Server and Agent
    sudo vi /etc/yum.repos.d/cloudera-manager.repo

* Add file corresponding to the CM version to install. In this case 5.10.2
    
```
[cloudera-manager]
# Packages for Cloudera Manager, Version 5, on RedHat or CentOS 7 x86_64
name=Cloudera Manager
baseurl=https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/5/
gpgkey =https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/RPM-GPG-KEY-cloudera
gpgcheck = 1
```

* Install the cloudera manager `sudo yum install cloudera-manager-server cloudera-manager-daemons`
* Install the cloudera manager `sudo yum install cloudera-manager-agent`

### 12. Prepare the Cloudera Manager DB
    cd /usr/share/cmf/schema/
    sudo ./scm_prepare_db.sh <cm_database> <cm_username> <cm_password>
    
### 13. Start Cloudera Manager server
    sudo systemctl start cloudera-scm-server
    
**Output:**
```
cloudera-scm-server.service - LSB: Cloudera SCM Server
Loaded: loaded (/etc/rc.d/init.d/cloudera-scm-server; bad; vendor preset: disabled)
Active: active (exited) since Tue 2017-07-18 09:28:20 EDT; 2h 47min ago
```

### 14. Start Cloudera Manager agent 
    sudo systemctl start cloudera-scm-agent

**Output:**
```
cloudera-scm-agent.service - LSB: Cloudera SCM Agent
Loaded: loaded (/etc/rc.d/init.d/cloudera-scm-agent; bad; vendor preset: disabled)
Active: active (exited) since Tue 2017-07-18 09:28:16 EDT; 2h 48min ago
```
