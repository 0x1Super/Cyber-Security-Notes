# nmap scan
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.1 (Ubuntu Linux; protocol 2.0)
53/tcp open  domain  syn-ack ISC BIND 9.10.3-P4 (Ubuntu Linux)
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
# gobuster scan
found admin.cronos.htb domain 
visiting the site we found a login page 

# dig 
doing 
dig axfr @$IP cronos.htb
gives us 
![[Pasted image 20210930015239.png]]

![[Pasted image 20210930021905.png]]
127.0.1.1       cronos.hackthebox.gr    cronos
