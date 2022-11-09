# nmap scan [[04 Studies/Tools/Nmap]]
22/tcp    open  ssh        syn-ack OpenSSH 4.3 (protocol 2.0)
25/tcp    open  smtp?      syn-ack
110/tcp   open  pop3?      syn-ack
111/tcp   open  rpcbind    syn-ack 2 (RPC #100000)
143/tcp   open  imap?      syn-ack
443/tcp   open  ssl/https? syn-ack
993/tcp   open  imaps?     syn-ack
995/tcp   open  pop3s?     syn-ack
3306/tcp  open  mysql?     syn-ack
4445/tcp  open  upnotifyp? syn-ack
10000/tcp open  http       syn-ack MiniServ 1.570 (Webmin httpd)
 
 running gobuster 
 
 ![[Pasted image 20210924163353.png]]
 and on recordings directory 
 ![[Pasted image 20210924163412.png]]
 [[pop]]
 POP3 v2.3.7
 connecting to smtp via telnet $IP 25 
 VRFY root
252 2.0.0 root
FreePBX 2.8.1.4 on 10.129.1.226
let's enumerate elastix and freepbx more 
using searchsploit to see if there is any elastix exploits
and we found a [[LFI]]  [[00 Progress/Boxes/HTB/Linux/Beep/01 Exploit]]






