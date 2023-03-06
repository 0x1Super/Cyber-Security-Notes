# Enumeration 

## open ports


```
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux; protocol 2.0)                    
| ssh-hostkey:                                                     
|   1024 08eed030d545e459db4d54a8dc5cef15 (DSA)                    
|   2048 b8e015482d0df0f17333b78164084a91 (RSA)                                                                                        
|   256 a04c94d17b6ea8fd07fe11eb88d51665 (ECDSA)                   
|_  256 2d794430c8bb5e8f07cf5b72efa16d67 (ED25519) 
53/tcp open  domain  ISC BIND 9.9.5-3ubuntu0.14 (Ubuntu Linux)
| dns-nsid:                
|_  bind.version: 9.9.5-3ubuntu0.14-Ubuntu
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: OPTIONS GET HEAD POST
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

### Port 53 Trying zone transfer
![[Pasted image 20230124050406.png]]

![[Pasted image 20230124050821.png]]
default page
### Port 80
![[Pasted image 20230124061442.png]]
sending me to default Debian page 
guessing domain to be bank.htb
![[Pasted image 20230124050346.png]]

/assets is accessible 
Nothing in there

/balance-transfer
![[Pasted image 20230124061548.png]]


![[Pasted image 20230124061607.png]]
Inside each file encrypted creds
downloaded the directory using wget
`wget --recursive http://bank.htb/balance-transfer`
using `ls -laS` to sort by size
![[Pasted image 20230124061715.png]]
and we have valid creds
![[Pasted image 20230124061936.png]]

# Foothold 

![[Pasted image 20230124061958.png]]

![[Pasted image 20230124073103.png]]
found comment that says .htb extension gets executed
renamed our php rev shell to rev.htb
![[Pasted image 20230124073245.png]]

# Privesc www -> root

found suid that when ran we become root?
![[Pasted image 20230124073428.png]]


# Loot

chris website creds
	username:crhis@bank.htb
	password:  `!##HTBB4nkP4ssw0rd!##`



---
Tags: #ctf #htb
Resources: [Bank](https://app.hackthebox.com/machines/26)