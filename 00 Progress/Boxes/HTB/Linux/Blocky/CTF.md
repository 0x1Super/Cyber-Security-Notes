
# 1. Enum

nmap scan

 8192/tcp closed sophos
 80/tcp  Apache httpd 2.4.18
 22/tcp    OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
 21/tcp   open   ftp?
 25565/tcp open   minecraft

running gobuster
found that website is using WordPress and phpmyadmin
## 1.1 jar files
found /plugins
![[Pasted image 20221119155504.png]]
using jd-gui we can decompile the .jar file 
![[Pasted image 20221119160217.png]]
![[Pasted image 20221119160231.png]]
trying the the password we just got on WordPress and phpmyadmin trying the  usernames notch, admin and root and we are in phpmyadmin as admin and also logged in as notch through ssh
# 2. Foothold
## SSH

![[Pasted image 20221119171406.png]]

<br><br><br><br><br><br>

<br><br><br><br><br><br><br>
## phpmyadmin (WordPress)
![[Pasted image 20221119160654.png]]
![[Pasted image 20221119160719.png]]
Wordpress notch user password


we can generate new password for the user notch and change it using

```bash
php -a
```
```php
echo password_hash('password', PASSWORD_DEFAULT);

```
```
# Output
$2y$10$BN3cjEENvAFmrlnSP5wugeimH9ncRwD.SnvYQjqeBjvdEXGYd3iA.
```
or 
https://codebeautify.org/wordpress-password-hash-generator
![[Pasted image 20221119163340.png]]
![[Pasted image 20221119163401.png]]
![[Pasted image 20221119163550.png]]

now we can try to upload php reverse shell into Apperance -> Editor and get a reverse shell on the target 

![[Pasted image 20221119164425.png]]
![[Pasted image 20221119164547.png]]
and paste our rev shell here
https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php
change
![[Pasted image 20221119183643.png]]
and then update

http://blocky.htb/wp-content/themes/twentyseventeen/404.php
![[Pasted image 20221119164911.png]]
![[Pasted image 20221119164954.png]]
```bash
which python
which python3
python3 -c 'import pty; pty.spawn("/bin/bash")'
CTRL + Z
stty raw -echo 
fg
enter
enter
export TERM=xterm
```

<br><br><br><br><br><br>

<br><br><br><br><br><br><br>
## FTP 
and we have notch's home directory 
![[Pasted image 20221119173655.png]]

we can try to steal ssh key
![[Pasted image 20221119173849.png]]
because there is no .ssh we can create it and copy our public ssh key to his directory and  login with ssh 


![[Pasted image 20221119174443.png]]
![[Pasted image 20221119174516.png]]
and we are in without a password




## 2.2Finding notch password (with www-data)
upload linpeas to the target
![[Pasted image 20221119165221.png]]
and we let it run 

![[Pasted image 20221119173315.png]]
![[Pasted image 20221119173242.png]]
and we can switch user to notch 





# 3. Privesc

with notch user we can run sudo -l 

![[Pasted image 20221119174918.png]]
and we have sudo perm on everything
![[Pasted image 20221119174937.png]]

## 3.1 Privesc (with FTP)
![[Pasted image 20221119181119.png]]
![[Pasted image 20221119175151.png]]
and we have wordpress password and we can go back to step step 2 and change notch user password

# CREDS founds
| Service | Username | password |
| ------- | -------- | -------- |
|    ssh     |   notch     |  8YsqfCTnvxAUeduzjNSXe22   |
|    ftp   |   notch   |  8YsqfCTnvxAUeduzjNSXe22  |
| phpmyadmin|  root    |   8YsqfCTnvxAUeduzjNSXe22   |
|phpmyadmin|  wordpress  |    kWuvW2SYsABmzywYRdoD  |
