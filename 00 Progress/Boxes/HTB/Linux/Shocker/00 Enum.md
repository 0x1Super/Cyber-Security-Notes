# Nmap 
80/tcp   open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
2222/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
# gobuster
got /cgi-bin/
running go buster again with -x pl,sh,php
we find user.sh which downloads a file


