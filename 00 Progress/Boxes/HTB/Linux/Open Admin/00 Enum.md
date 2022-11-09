IP =10.129.188.240

First as usual we start with our port scanning and hosts file
export IP=10.129.188.240 
sudo vim /etc/hosts
# Port scan 
nmap -p- $IP -vvv -oA nmap/nmap.all
nmap -p -sC -sV 22,80 $IP -vvv -oA nmap/nmap.det
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))



port 80 is open 
checking the page 
and running gobuster in the background
gobuster dir -u openadmin.htb -w /opt/Seclists/Discovery/Web-Content/raft-medium-directories.txt 
we found /music in gobuster that looks interesting
![[Pasted image 20211211205056.png]]
Music | NOT LIVE/NOT FOR PRODUCTION USE
found a login page 
![[Pasted image 20211211212831.png]]

it tells us that  the opennetadmin version is v18.1.1

and artwork
![[Pasted image 20211211205154.png]]
/sierra
![[Pasted image 20211211210057.png]]

found potential plugins in source code
Colorlib
Owl Carousel v2.3.4