IP = 10.129.191.27

First of all we need to check what ports and services are running on the target machine and we can do so by running nmap scan first to scan all ports then with scripts to enumerate the version and services running on the target 
# Port scan
first let's export the ip to make it faster to type
export IP=10.129.191.27
nmap -p- $IP -vvv -oA nmap/nmap.all
![[Pasted image 20211208152907.png]]
now let's do 
nmap -p 22,80,8065 $IP -sC -sV -vvv -oA nmap/nmap.det
80/tcp   open  http    syn-ack nginx 1.14.2                                                                                                                   
22/tcp   open  ssh     syn-ack OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)                                         8065/tcp open  unknown syn-ack           
                                        





Looking at the results port 80/http is open now let's dig into the website and see what we can find 
first let's add the target to the hosts file![[Pasted image 20211208151430.png]]
and before we start let's run gobuster in the background to have some enumeration running in the background 
gobuster dir -u delivery.htb -w /opt/Seclists/Discovery/Web-Content/raft-medium-directories.txt -x txt 
![[Pasted image 20211208151805.png]]
in contact us there is a link to a helpdesk and a login page in mattermost server on port 8065
![[Pasted image 20211208151852.png]] 
and we need a delviry.htb account to access mattermost
in help desk we need to add the subdomain into the hosts files 
![[Pasted image 20211208161408.png]] and looks like it's working with osTicket 
and there is a login page 
![[Pasted image 20211208152332.png]]
from gobuster scan we found another login in 
helpdesk.delivery.htb/scp/login.php
![[Pasted image 20211208153517.png]]
Doing searchsploit on osticket to try to see if there is a way to enumerate version 
![[Pasted image 20211208161308.png]]
there is a couple of exploits but we don't really know the version of osticket
after getting to dead end on trying to get the osticket version will go back to try to poke into the website more 
Now trying to submit a ticket to the server and see if i can get anything out of it and i can try to send a link to see if it's possible that it may be clicked by someone now let's open nc listener 
![[Pasted image 20211208162431.png]] 
looks like we got a ticket id and a email with the domain delivery.htb 
![[Pasted image 20211208162637.png]]
now let's try to login and see what we can do with this email 
first let's login with the email we submitted earlier in the check ticket status with this ticket with the ticket id we got 
![[Pasted image 20211208162707.png]]
now we have  a email from same domain as the server we can try to bypass authentication by creating an account on mattermost that we found earlier because it requiers only delivery.htb emails and the verification code should be sent to the ticket inbox  ![[Pasted image 20211208163152.png]]
and we have it!
![[Pasted image 20211208163213.png]]
a verification email to the ticket email we found earlier 
now we can go to this website to activate my acc 
![[Pasted image 20211208163313.png]]
![[Pasted image 20211208163331.png]]

