as usual let's get stable shell
first we do which python found nothing then which python3 found python3 on the target so 
python3 -c 'import pty;pty.spawn("/bin/bash")'
CTRL Z 
stty raw -echo
fg enter enter
TERM=xterm
now we have stable shell on the box
let's start with our privesc
found users 
root
jimmy 
joanna
found db creds in /var/www/ona/local/config/database_settings.inc.php
it''s mySQL
![[Pasted image 20211211220643.png]]
using creds we just found we are in the database 
![[Pasted image 20211212130928.png]]
![[Pasted image 20211212131729.png]]
we have opennetadmin hash
![[Pasted image 20211212131901.png]]
![[Pasted image 20211212131936.png]]
what we can do now is get the creds we found so far and usernames and brute force ssh with it because ssh is open
using hydra we found a valid creds for jimmy
![[Pasted image 20211212133054.png]]
now let's ssh to the target using jimmy creds 
jimmy:n1nj4W4rri0R!
![[Pasted image 20211212133159.png]] 
we are in as jimmy
Linux version 4.15.0-70-generic (buildd@lgw01-amd64-055) (gcc version 7.4.0 (Ubuntu 7.4.0-1ubuntu1~18.04.1)) # 79-Ubuntu SMP 
Sudo version 1.9.7p1
/home/jimmy/.local/share/nano/search_history(nothing)
/var/www/internal we found 
![[Pasted image 20211212135621.png]]
which probably means that if we open this site we would get ssh key for joanna
running ss -lntp to check for open ports we found a weird port at 52846 and it's probably for the internal site

using curl to get the site content
![[Pasted image 20211212135902.png]]
and we have ssh key but it looks like it's encrypted so we can use john the ripper to crack it's password
first we need to get it into format for john to be able to crack it 
ssh2john id_rsa > id_rsa.john
now we can crack it 
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa.john 
and we have a password
![[Pasted image 20211212141348.png]]
joanna:bloodninjas
first let's change the encrypted key perm to fit 
chmod 600 id_rsa
now let's ssh
ssh joanna@openadmin.htb -i id_rsa
when it assks for a password for the key it's what we just found bloodninjas
#userflag 8abe6d8b101f9ff8a6334e8819125f2d
running sudo -l we found that we can run sudo on nano with a priv bin 
![[Pasted image 20211212143959.png]]
so now we can check https://gtfobins.github.io/gtfobins/nano/
![[Pasted image 20211212144028.png]]
![[Pasted image 20211212144413.png]]
![[Pasted image 20211212144500.png]]
and we are root
#rootflag 9a72ef55a53d2ad2fa409b70624763d0
