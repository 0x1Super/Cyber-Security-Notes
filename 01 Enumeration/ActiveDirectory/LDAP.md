
ldapsearch -h IP -x -s SCOPE FILTER 



ldapsearch -h 10.129.95.210 -x -b base namingcontexts
ldapsearch -h 10.129.95.210 -x -b "DC=htb,DC=local" > ldap.out to dump info from ldap server then we can try to query using 
ldapsearch -h 10.129.95.210 -x -b "DC=htb,DC=local" '(object Class=Person)'