### Database server's 

    hostname

**Output:** `ip-172-31-34-86.us-west-2.compute.internal`

### Database server version

    >SELECT VERSION();

**Output:** `5.5.52-MariaDB`

### Avaialble databases

    >SHOW databases;
	
**Output:**
```
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
13 rows in set (0.00 sec)
```
