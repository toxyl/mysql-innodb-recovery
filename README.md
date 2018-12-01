# MySQL InnoDB Databases Recovery Tool
A shell script that helps to recover corrupted MySQL databases (InnoDb engine).

# How To Use
Copy the script to `/usr/local/bin` and make it executable (`chmod +x mysql-innodb-recovery`). Navigate to the folder where you want to store your dump and run `mysql-innodb-recovery`:

```bash
cp mysql-innodb-recovery /usr/local/bin
chmod +x /usr/local/bin/mysql-innodb-recovery
cd /home/Desktop
mysql-innodb-recovery my_db_prefix /var/lib/mysql.damaged
```

The above script would recover the databases that are named `my_db_prefix*` from the source directory (`/var/lib/mysql.damaged`). Besides the folders with the database specific files the script requires a few more files from the source directory: 
- `ibdata1`
- `mysql/gtid_executed.ibd`
- `mysql/innodb_index_stats.ibd` 
- `mysql/innodb_table_stats.ibd` 

Absence of these files will cause the script to terminate with an error.

# Motivation
This script is a radical approach born out of frustration. After painstakingly piecing together a set of databases from a bunch of database exports from various project stages (different table definitions, different data, you name it) my MySQL server crashed together with the whole machine. The usual recovery approaches I found online did not work, so I began combining approaches until I had a working dump.

# It's on you!
This is to say that I have **not** tested the script with other corrupt databases or system configurations different to mine (Ubuntu 18.04.1 LTS, MySQL 5.7.24). Use it at **YOUR OWN RISK** and don't forget to make a backup of your MySQL client and server files as they will be removed during the process. This includes database files and configurations. After using the tool you might want to consider reinstalling MySQL before importing the dump.  
