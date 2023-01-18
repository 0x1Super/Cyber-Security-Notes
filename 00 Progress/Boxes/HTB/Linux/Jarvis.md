# Enumeration 

## open ports
```
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 03f34e22363e3b813079ed4967651667 (RSA)
|   256 25d808a84d6de8d2f8434a2c20c85af6 (ECDSA)
|_  256 77d4ae1fb0be151ff8cdc8153ac369e1 (ED25519)
80/tcp open  http    Apache httpd 2.4.25 ((Debian))
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Stark Hotel
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
```


### Port 80

![[Pasted image 20230114154614.png]]

/phpmyadmin

![[Pasted image 20230114154624.png]]


# Foothold 



# Privesc 
# Loot



---
Tags: #ctf #htb 
Resources: [Jarvis](https://app.hackthebox.com/machines/194)