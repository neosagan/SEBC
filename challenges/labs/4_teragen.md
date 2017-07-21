### Teragen command
```
time hadoop jar /opt/cloudera/parcels/CDH-5.10.2-1.cdh5.10.2.p0.5/jars/hadoop-mapreduce-examples-2.6.0-cdh5.10.2.jar teragen -Dmapreduce.job.maps=8 -Dmapreduce.map.memory.mb=32 -Dmapreduce.map.java.opts.max.heap=512 65536000 /user/saturn/tgen
```
### Teragen time
```
real    1m13.128s
user    1m24.553s
sys     0m5.344s
```

### Output `hdfs dfs -ls /user/saturn/tgen`
```
-rw-r--r--   3 saturn planets          0 2017-07-19 22:11 /user/saturn/tgen/_SUCCESS
-rw-r--r--   3 saturn planets 6553600000 2017-07-19 22:11 /user/saturn/tgen/part-m-00000
```
### Output `hadoop fsck -blocks /user/saturn`
```
Connecting to namenode via http://ip-172-31-44-49.us-west-2.compute.internal:50070
FSCK started by saturn (auth:SIMPLE) from /172.31.34.86 for path /user/saturn at Wed Jul 19 22:14:06 UTC 2017
..Status: HEALTHY
 Total size:    6553600000 B
 Total dirs:    2
 Total files:   2
 Total symlinks:                0
 Total blocks (validated):      196 (avg. block size 33436734 B)
 Minimally replicated blocks:   196 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     3.0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Number of data-nodes:          3
 Number of racks:               1
FSCK ended at Wed Jul 19 22:14:06 UTC 2017 in 9 milliseconds


The filesystem under path '/user/saturn' is HEALTHY
```
