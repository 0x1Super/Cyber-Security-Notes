# nmap scan
the port scanning showed that port 80 is open 
80/tcp open  http    syn-ack Apache httpd 2.2.15 ((CentOS) DAV/2 PHP/5.3.3)
OS : CentOS 
PHP: 5.3.3

# website enum 
 using gobuster shows that 
 there is robots.txt
  contains 
  : User-agent: *
Disallow: /cola
Disallow: /sisi
Disallow: /beer
checking beer
![[Pasted image 20211028151253.png]]xd?
/sisi 
same pic
/cola 
same pic e.e
trying /fristi cos of name of the box and we got a admin login panel
![[Pasted image 20211028153243.png]]