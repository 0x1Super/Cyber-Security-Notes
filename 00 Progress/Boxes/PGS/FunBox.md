# Enumeration 
```
21/tcp open  ftp     ProFTPD                                                                         
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)                             
| ssh-hostkey:                                                                                       
|   3072 d2:f6:53:1b:5a:49:7d:74:8d:44:f5:46:e3:93:29:d3 (RSA)                                       
|   256 a6:83:6f:1b:9c:da:b4:41:8c:29:f4:ef:33:4b:20:e0 (ECDSA)                                      
|_  256 a6:5b:80:03:50:19:91:66:b6:c3:98:b8:c4:4f:5c:bd (ED25519)                                    
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-robots.txt: 1 disallowed entry 
|_/secret/
|_http-title: Did not follow redirect to http://funbox.fritz.box/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

## Port 80


![[Pasted image 20230220220800.png]]


![[Pasted image 20230220221336.png]]
![[Pasted image 20230220221412.png]]
![[Pasted image 20230220221551.png]]
`wpscan --url http://funbox.fritz.box/ -U joe -P /usr/share/wordlists/rockyou.txt`
![[Pasted image 20230220222641.png]]
![[Pasted image 20230220222651.png]]

 Current administration email: funny@funbox.box	
 ![[Pasted image 20230220222811.png]]
![[Pasted image 20230220223149.png]]

![[Pasted image 20230220223137.png]]
![[Pasted image 20230220223255.png]]

# Foothold 

ssh
joe:12345

![[Pasted image 20230220223402.png]]
![[Pasted image 20230220223440.png]]

![[Pasted image 20230220223713.png]]
![[Pasted image 20230220223900.png]]


![[Pasted image 20230220223911.png]]

![[Pasted image 20230220223953.png]]

![[Pasted image 20230220224401.png]]




# Privesc 
# Conclusion



---
Tags:
Resources: [Proving Grounds](https://portal.offensive-security.com/labs/play) 