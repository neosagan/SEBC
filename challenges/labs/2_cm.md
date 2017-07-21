### Cloudera Manager 5.10.2 repo

    ls /etc/yum.repos.d

**Output:**
```
CentOS-Base.repo       CentOS-fasttrack.repo  CentOS-Vault.repo
CentOS-CR.repo         CentOS-Media.repo      cloudera-manager.repo
CentOS-Debuginfo.repo  CentOS-Sources.repo    mariadb.repo
```

### Execution of `scm_prepare_database.sh`
    sudo ./scm_prepare_database.sh mysql <database> <username> <password>
    sudo ./scm_prepare_database.sh mysql scm scm scm

**Output:**
```
JAVA_HOME=/usr/lib/jvm/java-openjdk
Verifying that we can write to /etc/cloudera-scm-server
Creating SCM configuration file in /etc/cloudera-scm-server
Executing:  /usr/lib/jvm/java-openjdk/bin/java -cp /usr/share/java/mysql-connector-java.jar:/usr/share/java/oracle-connector-java.jar:/usr/share/cmf/schema/../lib/* com.cloudera.enterprise.dbutil.DbCommandExecutor /etc/cloudera-scm-server/db.properties  com.cloudera.cmf.db.
[main] DbCommandExecutor              INFO  Successfully connected to database.
All done, your SCM database is configured correctly!
```
