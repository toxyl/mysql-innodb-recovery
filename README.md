# mysql-innodb-recovery
A shell script that helps to recover corrupted MySQL databases (InnoDb engine).

# How to?!?
Simple. Copy the script to `/usr/local/bin` and make it executable (`chmod +x mysql-innodb-recovery`). Navigate to the folder where you want to store your dump and run `mysql-innodb-recovery`. 
```bash
cp mysql-innodb-recovery /usr/local/bin
chmod +x /usr/local/bin/mysql-innodb-recovery
cd /home/Desktop
mysql-innodb-recovery my_db_prefix /var/lib/mysql.damaged
```
The above script would dump the databases that are named `my_db_prefix*` and located in `/var/lib/mysql.damaged`. Note that the script will also require the files `ibdata1`, `mysql/gtid_executed.ibd`, `mysql/innodb_index_stats.ibd` and `mysql/innodb_table_stats.ibd` files. 
