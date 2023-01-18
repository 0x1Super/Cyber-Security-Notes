# Enumeration 
10.129.106.239
Debian
## open ports
```
22/tcp  open  ssh      OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)                                                                   
| ssh-hostkey:                                                                                                                         
|   3072 df17c6bab18222d91db5ebff5d3d2cb7 (RSA)                                                                                        
|   256 3f8a56f8958faeafe3ae7eb880f679d2 (ECDSA)                                                                                       
|_  256 3c6575274ae2ef9391374cfdd9d46341 (ED25519)                                                                                     
80/tcp  open  http     Apache httpd 2.4.54                                                                                             
|_http-title: Did not follow redirect to https://broscience.htb/                                                                       
|_http-server-header: Apache/2.4.54 (Debian)                                                                                           
| http-methods:                                                                                                                        
|_  Supported Methods: GET HEAD POST OPTIONS                                                                                           
443/tcp open  ssl/http Apache httpd 2.4.54 ((Debian)) 
```

### Port 80


![[Pasted image 20230114155622.png]]
Redirect

### Port 443

![[Pasted image 20230114155711.png]]



![[Pasted image 20230114161747.png]]
![[Pasted image 20230114162014.png]]
![[Pasted image 20230114162028.png]]
![[Pasted image 20230114162041.png]]


![[Pasted image 20230114173505.png]]

# Foothold 



# Privesc 
# Loot



---
Tags: #ctf #htb 
Resources: 