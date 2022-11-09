first we start with our port scan 
# Port scan
nmap -p- -vvv -oA nmap/nmap.all $IP
nmap -p 22,80,6379,10000  -sC -sV $IP -oA nmap/nmap.det 
10000/tcp open  http    syn-ack MiniServ 1.910 (Webmin httpd)
6379/tcp  open  redis   syn-ack Redis key-value store 4.0.9
80/tcp    open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
22/tcp    open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)                                                                                                  



running gobuster for directory scan

port 80 enum
postman@htb
![[Pasted image 20211216194513.png]]
Didn't find much in the main page
checking other ports we found like webmin port 10000
it has to be https so we change the url 
![[Pasted image 20211216201759.png]]
and we have a webmin login page
![[Pasted image 20211216201815.png]]

port 6379 enum 
using redis-cli tool
info
run_id:ecc5b74710f43d507a2c04df68c02b3e33883cdd
os:Linux 4.15.0-58-generic x86_64
redis_version:4.0.9
![[Pasted image 20211216204805.png]]

