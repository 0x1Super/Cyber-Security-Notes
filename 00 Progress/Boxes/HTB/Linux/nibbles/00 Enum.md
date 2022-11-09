# nmap scan
 22/tcp    open     ssh         syn-ack     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
 80/tcp    open     http        syn-ack     Apache httpd 2.4.18 ((Ubuntu))
 
 41/tcp   filtered uucp-rlogin no-response
1974/tcp  filtered drp         no-response
2811/tcp  filtered gsiftp      no-response
3128/tcp  filtered squid-http  no-response
3333/tcp  filtered dec-notes   no-response
4567/tcp  filtered tram        no-response
5000/tcp  filtered upnp        no-response
9535/tcp  filtered man         no-response
38292/tcp filtered landesk-cba no-response
60443/tcp filtered unknown     no-response

checking the page source we found 
![[Pasted image 20210923151117.png]]
running gobuster on /nibbleblog/ we got some interesting stuff
# gobuster
![[Pasted image 20210923151321.png]]
![[Pasted image 20210923151335.png]]
in http://10.129.226.89/nibbleblog/README
we found 
====== Nibbleblog ======
Version: v4.0.3
Codename: Coffee
Release date: 2014-04-01
![[Pasted image 20210923151811.png]]


 