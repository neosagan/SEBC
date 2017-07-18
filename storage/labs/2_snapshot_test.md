### 1. Create directory
    hdfs dfs -mkdir /home/neosagan/precious

**Output:**
```
Found 5 items
drwx------   - neosagan neosagan          0 2017-07-18 17:07 /user/neosagan/.Trash
drwx------   - neosagan neosagan          0 2017-07-18 16:19 /user/neosagan/.staging
drwxr-xr-x   - neosagan neosagan          0 2017-07-18 16:11 /user/neosagan/output-teragen
drwxr-xr-x   - neosagan neosagan          0 2017-07-18 16:19 /user/neosagan/output-terasort
drwxr-xr-x   - neosagan neosagan          0 2017-07-18 17:08 /user/neosagan/precious
```

### 2. Copy the SEBC file to HDFS
    hdfs dfs -copyFromLocal /home/neosagan/SEBC-master.zip /user/neosagan/precious
    
**Output:**
```
Found 1 items
-rw-r--r--   3 hdfs neosagan     474826 2017-07-18 17:08 /user/neosagan/precious/SEBC-master.zip
```

### 3. Enable the snapshots
    sudo -u hdfs hdfs dfsadmin -allowSnapshot /user/neosagan/precious

**Output:** `Allowing snaphot on /user/neosagan/precious succeeded`

### 4. Create the snapshot
    hdfs dfs -createSnapshot /user/neosagan/precious sebc-hdfs-test  
    
**Output:** `Created snapshot /user/neosagan/precious/.snapshot/sebc-hdfs-test

### 5. Delete file
    hdfs dfs rm /user/neosagan/precious/SEBC-master.zip
    
### 6. Recover deleted file
    hdfs dfs -cp /user/neosagan/precious/.snapshot/sebc-hdfs-test/SEBC-master.zip /user/neosagan/precious
