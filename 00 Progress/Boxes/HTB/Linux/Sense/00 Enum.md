# nmap 
443/tcp open  ssl/https? syn-ack
80/tcp  open  http       syn-ack lighttpd 1.4.35
let's run gobuster to see if there is hidden dir with -x php,txt to see txt and php filees as weell 
# gobuster scan
/changelog.txt 
![[Pasted image 20211004210614.png]]
/system-users 
we found potential  creds 
![[Pasted image 20211004210836.png]]
[[00 Progress/Boxes/HTB/Linux/Sense/04 Creds]]
searching for default pfsense default and we are in as rohit

# Version 
2.1.3-RELEASE (amd64)
built on Thu May 01 15:52:13 EDT 2014
FreeBSD 8.3-RELEASE-p16