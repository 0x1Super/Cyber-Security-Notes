# nmap scan
22/tcp  open  ssh     syn-ack OpenSSH 7.4p1 Debian 10+deb9u1 (protocol 2.0)
25/tcp  open  smtp?   syn-ack
80/tcp  open  http    syn-ack Apache httpd 2.4.25 ((Debian))
10/tcp open  pop3?   syn-ack
119/tcp open  nntp?   syn-ack

scanning all ports results in 
22/tcp   open  ssh                                                                                             
25/tcp   open  smtp                                                                                            
80/tcp   open  http                                                                                            
110/tcp  open  pop3                                                                                            
119/tcp  open  nntp                                                                                            
4555/tcp open  rsip  

trying to connect to port 4555 with ncat and we are in ![[Pasted image 20211106170148.png]]
trying some default creds and we are in with root:root [[00 Progress/Boxes/HTB/Linux/Solid State/04 Creds]]
doing listusers we get 
![[Pasted image 20211106170251.png]]
l
# 
webadmin@solid-state-security.com
HTML5
aj@lkn.io 
 @ajlkn
Apache/2.4.25 (Debian) 
JAMES SMTP Server 2.3.2