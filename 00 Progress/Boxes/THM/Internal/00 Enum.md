Tags:Internal 
When: [[2021-09-20]]

##Note:
# Enumeration 
First we start with our Nmap on all ports nmap -p- -T4 $IP -vv -oA nmap/Allports.log
and we start with our gobuster scan cos there is a port 80 open and changing host file to add the file to check for subdomains as well 
visiting the website we get the default apache web page and it's running on ubuntu 

 now that there is a wordpress on the server 
blog (Status: 301)
/wordpress (Status: 301) 
/javascript (Status: 301)
/phpmyadmin (Status: 301) a login page
/server-status (Status: 403)

let's try vhost 

Found: gc._msdcs.internal.thm (Status: 400) [Size: 422]
Found: _domainkey.internal.thm (Status: 400) [Size: 422]
Found: mailing._domainkey.sunnynews.internal.thm (Status: 400) [Size: 422]
Found: mailing._domainkey.info.internal.thm (Status: 400) [Size: 422]
Found: hallam_dev.internal.thm (Status: 400) [Size: 422]
Found: hallam_ad.internal.thm (Status: 400) [Size: 422]
Found: wm_j_b__ruffin.internal.thm (Status: 400) [Size: 422]
Found: 2609_n_www.internal.thm (Status: 400) [Size: 422]
Found: 0907_n_hn.m.internal.thm (Status: 400) [Size: 422]
Found: 0507_n_hn.internal.thm (Status: 400) [Size: 422]
Found: faitspare_mbp.cit.internal.thm (Status: 400) [Size: 422]
Found: sb_0601388345bc6cd8.internal.thm (Status: 400) [Size: 422]
Found: sb_0601388345bc450b.internal.thm (Status: 400) [Size: 422]
Found: api_portal_dev.internal.thm (Status: 400) [Size: 422]
Found: api_web_dev.internal.thm (Status: 400) [Size: 422]
Found: api_webi_dev.internal.thm (Status: 400) [Size: 422]
Found: sklep_test.internal.thm (Status: 400) [Size: 422]

nmap scan 
2/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 6e:fa:ef:be:f6:5f:98:b9:59:7b:f7:8e:b9:c5:62:1e (RSA)
|   256 ed:64:ed:33:e5:c9:30:58:ba:23:04:0d:14:eb:30:e9 (ECDSA)
|_  256 b0:7f:7f:7b:52:62:62:2a:60:d4:3d:36:fa:89:ee:ff (EdDSA)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works

[[01 Enumeration/Web/WordPress]]
running wpscan 
WordPress version 5.4.2 identified  (Insecure, released on 2020-06-10).
and there is 1 user discovered which is admin 
we cracked the password with wordpress crack tool 
now we are in wordpress 

[[00 Progress/Boxes/THM/Internal/01 Exploit]]



----------------------------------------------------------------------------------
