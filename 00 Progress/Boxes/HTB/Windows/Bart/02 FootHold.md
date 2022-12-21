inside Server monitor we found another domain 
http://internal-01.bart.htb/simple_chat/login_form.php 
which has another login prompt we can try to use [[Hydra]]for this one
it kept giving random passwords
hydra -l harvey -P /usr/share/wordlists/rockyou.txt 127.0.0.1 http-form-post "/simple_chat/login.php:uname=^USER^&passwd=^PASS^&submit=Login:Password"
to make this faster we ran it with /usr/share/wordlists/metasploit/common_roots.txt
and we got it Password1 [[00 Progress/Boxes/HTB/Windows/Bart/04 Creds]]
![[Pasted image 20210927183512.png]]
checking page source we see
![[Pasted image 20210927183924.png]]
checking http://internal-01.bart.htb/log/log.php?filename=log.txt&username=harvey
we find that it executes logs after filename= 
changing to http://internal-01.bart.htb/log/log.php?filename=log&username=harvey
[2021-09-27 19:50:15] - harvey - Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
user agent is vul
it executes user agent we can change that in burp to add a reverse shell
[[03 Commands/PHP]]
using [[Upload-Download files]]to upload our shell
well after spending literally 1 day not knowing why my shell never gets executed WELL i was copying the nishang Invoke line with the PS > for god sake 
anyway leet's try again after fixing it 
we are in!!
![[Pasted image 20210928154227.png]]