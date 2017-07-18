### 1. Create user account in all nodes
    sudo groupadd neosagan
    sudo useradd -g neosagan neosagan
    sudo passwd neosagan
    
* Check the successful creation `id neosagan`

**Output:** `uid=1001(neosagan) gid=1001(neosagan) groups=1001(neosagan)`

* Repeat the previous procedure on all nodes.

### 2. Create a HDFS directory /user
    sudo -u hdfs hdfs dfs -mkdir /user/neosagan
    sudo -u hdfs hdfs dfs -chown -R neosagan:neosagan /user/neosagan
    su neosagan
    hdfs dfs -ls /user

**Output:**
```
Found 4 items
drwxrwxrwx   - mapred   hadoop            0 2017-07-18 09:58 /user/history
drwxrwxr-t   - hive     hive              0 2017-07-18 10:03 /user/hive
drwxrwxr-x   - hue      hue               0 2017-07-18 10:04 /user/hue
drwxr-xr-x   - neosagan neosagan          0 2017-07-18 13:58 /user/neosagan
``` 
### 3. Benchmarking with teragen
    time hadoop jar hadoop-mapreduce-examples-2.6.0-cdh5.12.0.jar teragen -Ddfs.blocksize=32M -Dmapred.map.tasks=4 100000000 /user/neosagan/output-teragen

**Output:** 
```
Found 5 items
-rw-r--r--   3 neosagan neosagan          0 2017-07-18 15:38 /user/neosagan/output/_SUCCESS
-rw-r--r--   3 neosagan neosagan 2500000000 2017-07-18 15:38 /user/neosagan/output/part-m-00000
-rw-r--r--   3 neosagan neosagan 2500000000 2017-07-18 15:38 /user/neosagan/output/part-m-00001
-rw-r--r--   3 neosagan neosagan 2500000000 2017-07-18 15:38 /user/neosagan/output/part-m-00002
-rw-r--r--   3 neosagan neosagan 2500000000 2017-07-18 15:38 /user/neosagan/output/part-m-00003
```

**Time (4 mappers):**
```
real    1m37.824s
user    0m5.959s
sys     0m0.225s
```

### 4. Benchmarking with terasort
    time hadoop jar hadoop-mapreduce-examples-2.6.0-cdh5.12.0.jar terasort -Ddfs.blocksize=32M -Dmapred.map.tasks=4 /user/neosagan/output-teragen /user/neosagan/output-terasort
    
**Output:**
```
Found 8 items
-rw-r--r--   1 neosagan neosagan          0 2017-07-18 16:19 /user/neosagan/output-terasort/_SUCCESS
-rw-r--r--  10 neosagan neosagan         55 2017-07-18 16:13 /user/neosagan/output-terasort/_partition.lst
-rw-r--r--   1 neosagan neosagan 1657210500 2017-07-18 16:18 /user/neosagan/output-terasort/part-r-00000
-rw-r--r--   1 neosagan neosagan 1669051200 2017-07-18 16:19 /user/neosagan/output-terasort/part-r-00001
-rw-r--r--   1 neosagan neosagan 1654375500 2017-07-18 16:19 /user/neosagan/output-terasort/part-r-00002
-rw-r--r--   1 neosagan neosagan 1668843100 2017-07-18 16:19 /user/neosagan/output-terasort/part-r-00003
-rw-r--r--   1 neosagan neosagan 1681409800 2017-07-18 16:19 /user/neosagan/output-terasort/part-r-00004
-rw-r--r--   1 neosagan neosagan 1669109900 2017-07-18 16:19 /user/neosagan/output-terasort/part-r-00005
```

**Time (4 mappers):**
```
17/07/18 16:19:28 INFO terasort.TeraSort: done
real    6m27.868s
user    0m8.617s
sys     0m0.342s
```
