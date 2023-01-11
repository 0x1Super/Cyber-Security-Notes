# Enumeration 

```
22/tcp   open  ssh     syn-ack OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)                                                 
80/tcp   open  http    syn-ack Apache httpd 2.4.38 ((Debian))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.38 (Debian)
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/                   
|_http-title: Did not follow redirect to http://sunset-midnight/
3306/tcp open  mysql   syn-ack MySQL 5.5.5-10.3.22-MariaDB-0+deb10u1
```

after adding domain to /etc/hosts 

![[Pasted image 20230105191644.png]]
running on wordpress

```
wpscan --url sunset-midnight -e ap

[i] Plugin(s) Identified:                                                                                                     

[+] simply-poll-master                                                                                                        
 | Location: http://sunset-midnight/wp-content/plugins/simply-poll-master/                                                    
 |
 | Found By: Urls In Homepage (Passive Detection)
 | Confirmed By: Urls In 404 Page (Passive Detection)
 |
 | Version: 1.5 (100% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://sunset-midnight/wp-content/plugins/simply-poll-master/readme.txt
 | Confirmed By: Readme - ChangeLog Section (Aggressive Detection)
 |  - http://sunset-midnight/wp-content/plugins/simply-poll-master/readme.txt


wpscan --url sunset-midnight -e u # enumurate users


```











# Exploitation 

















# Foothold 















# Privesc 
# Conclusion



---
Tags:
Resources: [Proving Grounds](https://portal.offensive-security.com/labs/play) 