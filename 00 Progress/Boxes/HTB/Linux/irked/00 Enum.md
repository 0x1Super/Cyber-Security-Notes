# nmap scan

22/tcp  open  ssh     syn-ack OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)
80/tcp  open  http    syn-ack Apache httpd 2.4.10 ((Debian))
111/tcp open  rpcbind syn-ack 2-4 (RPC #100000)



running -p- on nmap 
found port 8067 open and it's IRC 
checking rfc irc on google https://datatracker.ietf.org/doc/html/rfc1459
![[Pasted image 20211027132832.png]]
server is running Unreal3.2.8.1
This server was created Mon May 14 2018 at 13:12:50 EDT
so let's test the backdoor we found 
https://lwn.net/Articles/392201/
![[Pasted image 20211027134129.png]]
![[Pasted image 20211027134141.png]]
and we get a icmp ping back 
now let's try to do a reverse shell [[Reverse Shells]]
![[Pasted image 20211027134309.png]]
and we are in!
![[Pasted image 20211027134318.png]]
