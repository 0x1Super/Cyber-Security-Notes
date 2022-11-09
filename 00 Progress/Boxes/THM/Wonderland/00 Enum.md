# nmap 
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    syn-ack Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
 # gobuster
 well this is fun first we get 
 /index.html           (Status: 301) [Size: 0] [--> ./]
/img                  (Status: 301) [Size: 0] [--> img/]
/r     
checking /r dir we found 
![[Pasted image 20210928182111.png]]
then we gobuster again on /r and we found /a
![[Pasted image 20210928182134.png]]
running gobuster again we get /b
![[Pasted image 20210928182215.png]]
running again /b
![[Pasted image 20210928182258.png]]
and again
/i 
![[Pasted image 20210928182339.png]]
and again
WELL it's rabbit 
xd so last one is 
/t
![[Pasted image 20210928182420.png]]
in last dir /r/a/b/b/i/t 
we found [[00 Progress/Boxes/THM/Wonderland/04 Creds]]on the source code
![[Pasted image 20210928184008.png]]
