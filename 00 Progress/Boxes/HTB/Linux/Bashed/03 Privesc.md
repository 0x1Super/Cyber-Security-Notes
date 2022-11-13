running sudo -l 

www-data@bashed:/var/www/html/dev# sudo -l

Matching Defaults entries for www-data on bashed:
env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on bashed:
(scriptmanager : scriptmanager) NOPASSWD: ALL

so we can basically run sudo -u scriptmanager and the command we want 
sudo -u scriptmanager /

sudo -u scriptmanager bash
and we get a bash as the usuer scriptmanager

running ls on / directory we find a weird directory which is scripts 

running ls -la we find out that test.py time always gets up to date which means
means there is a corn job probably running every minute and execute that file 
![[Pasted image 20210923144417.png]]
and we own that file so we can modify the commands inside
using payloadofallthethings python reverse shell 
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#python

![[Pasted image 20221111142613.png]]

and we got a shell as root
![[Pasted image 20221111142723.png]]


#rootflag cc4f0afe3a1026d402ba10329674a8e2