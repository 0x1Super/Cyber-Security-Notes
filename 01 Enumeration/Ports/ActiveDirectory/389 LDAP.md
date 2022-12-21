## Ldapsearch
```bash
# ldap query 
ldapsearch -x -H ldap://support.htb -D "support\\ldap" -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -b "DC=support,DC=htb" '(objectClass=Person)' # ldap query get ppl you can also use * to dumpall 
# anything after objectclass=person will be filtered 



ldapsearch -h 10.129.95.210 -x -b base namingcontexts
ldapsearch -h 10.129.95.210 -x -b "DC=htb,DC=local" > ldap.out # to dump info from ldap server then we can try to query using 
ldapsearch -h 10.129.95.210 -x -b "DC=htb,DC=local" '(object Class=Person)'

ldapsearch -H ldap://athos.host -D 'cn=admin,dc=athos,dc=host' \ -w 'p@ssw0rd' -x -LLL -b 'dc=athos,dc=host' 'dn'
ldapsearch -x -H ldap://support.htb  -w '<Password>' # connect to ldap check for creds
ldapsearch -D 'cn=admin,dc=athos,dc=host' -w '<Password>' # check for creds using -D

ldapsearch -x -H ldap://<IP> -D '<DOMAIN>\<username>' -w '<password>' -b "DC=<1_SUBDOMAIN>,DC=<TLD>"


ldapsearch ldapsearch -x -H ldap://<DOMAIN>  -w '<Password>' -s base namingcontexts # check for naming for our user


ldapsearch -x -H ldap://support.htb -D "support\\ldap" -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -b "DC=support,DC=htb" '(objectClass=*)' sAMAccountName | grep sAMAccountName # dump users

ldapsearch -x -H ldap://support.htb -D "support\\ldap" -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -b "CN=support,CN=Users,DC=support,DC=htb" # check user info

# eg.
ldapsearch -x -H ldap://support.htb  -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -s base namingcontexts # get name 
ldapsearch -x -H ldap://support.htb  -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -s sub -b "DC=support,DC=htb" # dump all
ldapsearch -x -H ldap://support.htb -D "support\\ldap" -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -b "DC=support,DC=htb" '(objectClass=Person)' # ldap query get ppl 

-x # Simple Authentication
-H # LDAP Server
-D # My User
-w # My password
-b # Base site, all data from here will be given

```


## Valid Credentials



```bash
pip3 install ldapdomaindump 
ldapdomaindump <IP> [-r <IP>] -u '<domain>\<username>' -p '<password>' [--authtype SIMPLE] --no-json --no-grep [-o /path/dir]
```