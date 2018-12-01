# MySQL InnoDB Databases Recovery Tool
A shell script that helps to recover corrupted MySQL databases (InnoDb engine).

# How To Use
Become root, copy the script to `/usr/local/bin` and make it executable (`chmod +x mysql-innodb-recovery`). Navigate to the folder where you want to store your dump and run `mysql-innodb-recovery [databases prefix] [damaged data dir]`.

## Example
```bash
su root
cp mysql-innodb-recovery /usr/local/bin
chmod +x /usr/local/bin/mysql-innodb-recovery
cd /root/Desktop
mysql-innodb-recovery my_db_prefix /var/lib/mysql.damaged
```

The above script would recover the databases that are named `my_db_prefix*` from the source directory `/var/lib/mysql.damaged` to `/var/lib/mysql` (*if your path is different you will have to adjust the hardcoded path in the script*). The dump will be saved to `/root/Desktop/my_db_prefix.dump.sql` Besides the folders with the database specific files the script requires a few more files from the source directory: 

- `ibdata1`
- `mysql/gtid_executed.ibd`
- `mysql/innodb_index_stats.ibd` 
- `mysql/innodb_table_stats.ibd` 

Absence of these files will cause the script to terminate with an error.

# Important!
The script will leave an insecure MySQL installation behind that is configured for local database recovery, **not** production server use! It is advised to reinstall MySQL to ensure proper security. Import your dump after reinstalling. 

# Motivation
This script is a radical approach born out of frustration. After painstakingly piecing together a set of databases from a bunch of database exports from various project stages (different table definitions, different data, you name it) my MySQL server crashed together with the whole machine. The usual recovery approaches I found online did not work, so I began combining approaches until I had a working dump.

# How It Works
To achieve that the script will completely remove MySQL (client and server) and all associated configuration files as well as stored databases. After removing MySQL it will clean up the OS and reinstall MySQL. When the reinstall is done the script will grant itself root access by modifying the `mysql` database using the `mysqld_safe` command. Now it will copy all files from the damaged databases and a few more that are needed to be able to repair the databases (namely `ibdata1`, `mysql/gtid_executed.ibd`, `mysql/innodb_index_stats.ibd`, `mysql/innodb_table_stats.ibd`). Then the script will start MySQL normally and first run `mysqlcheck` and then `mysqlanalyze` over the databases to recover. If everything went according to plan the script will now finally dump the databases using `mysqldump`. 

Errors during the process will cause the script to terminate and print out the last few lines of the `syslog` as well as `/var/log/mysql/error.log`. Those are not necessarily the last lines before the error, therefore you should first check the script's output above the `syslog` printout for error messages and if needed the `/var/log/mysql/error.log` file.

# Disclaimer
I have **not** tested the script with other corrupt databases or system configurations different to mine (Ubuntu 18.04.1 LTS, MySQL 5.7.24). Use it at **YOUR OWN RISK** and don't forget to make a backup of your MySQL client and server files as **they will be removed** during the process. This includes database files and configurations! 
