# nmap scan 
80/tcp open  http    syn-ack Microsoft IIS httpd 10.0
running gobuster gets us no where but running vhost gobuster we found some interesting stuff
found fourm.bart.htb after adding that to /etc/hosts we are able to see the page 
![[Pasted image 20210927164710.png]]
info@bart.htb
![[Pasted image 20210927165400.png]]
running wordpress

after running with more issues with gobuster i ran it with -b 200 to prevent it from showing 200 status code 
![[Pasted image 20210927172050.png]]
php server Monitor 3.2.1
