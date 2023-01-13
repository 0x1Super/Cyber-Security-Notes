# Enumeration 
10.129.228.112

Service Info: Host: JEEVES; OS: Windows; CPE: cpe:/o:microsoft:windows
Microsoft Windows 7 - 10 microsoft-ds 
## open ports

```
80/tcp    open  http         Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0                                                                                                         
| http-methods:                                                         
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE                                                                                                             
|_http-title: Ask Jeeves

135/tcp   open  msrpc        Microsoft Windows RPC
445/tcp   open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)

50000/tcp open  http         Jetty 9.4.z-SNAPSHOT
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
|_http-title: Error 404 Not Found


```

### Port 80

![[Pasted image 20230111210657.png]]


### Port 445
Anonymous login not permitted 


### Port 50000

![[Pasted image 20230111211313.png]]

# Foothold 



# Privesc 
# Loot



---
Tags: #ctf #htb #potato
Resources: [Jeeves](https://app.hackthebox.com/machines/Jeeves)