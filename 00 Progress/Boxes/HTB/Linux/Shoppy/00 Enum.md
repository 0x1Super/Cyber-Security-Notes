# **Open ports**

80/tcp http penSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0) nginx/1.23.1 
22/tcp ssh

![[Pasted image 20221111220206.png]]

/admin
![[Pasted image 20221111220345.png]]

couldn't do much i can try to run nmap to search for all ports
found a weird port open 
9093/tcp 
looks like it's running on something called copycat 
![[Pasted image 20221114201040.png]]

couldn't find much 
on background i was running wfuzz to look for subdomains 
![[Pasted image 20221114232614.png]]

mattermost.shoppy.htb
![[Pasted image 20221114232746.png]]
back to /login page and attempt to do SQLI against it 
using 
https://book.hacktricks.xyz/pentesting-web/nosql-injection
we managed to bypass login using 
```
Mongo sql: '||' 1==1// or '||' 1==1%00
```

![[Pasted image 20221114203851.png]]
![[Pasted image 20221114203913.png]]

trying same command on search bar 
![[Pasted image 20221114224812.png]]
![[Pasted image 20221114224825.png]]
looks like we have here creds for admin and josh user
cracking the hashes and we got josh's password

josh:remembermethisway

trying josh username:password to login ssh but was unsuccessful that leaves us with mattermost subdomain that we found earlier 
and we are in 
![[Pasted image 20221114232810.png]]
![[Pasted image 20221114232834.png]]
![[Pasted image 20221114233109.png]]
jaeger:Sh0ppyBest@pp!
looks like it's SSH creds
![[Pasted image 20221114233518.png]]
and we are in as the user jaeger

#userflag bce442d36f2422d5f041f02062369493
![[Pasted image 20221114233640.png]]
looks like it's a script that gives us a password if we wrote the right master password
![[Pasted image 20221114235114.png]]
well looks like if u cat the file 
![[Pasted image 20221114235155.png]]

it tells u what the password is

![[Pasted image 20221114235213.png]]

deploy:Deploying@pp!

![[Pasted image 20221114235243.png]]
![[Pasted image 20221115000324.png]]
user deploy is in docker group which can lead to priv esc to root 
https://gtfobins.github.io/gtfobins/docker/

```
docker run -v /:/mnt --rm -it alpine chroot /mnt sh

```


![[Pasted image 20221115000545.png]]

#rootflag 0bb105bea98ce2b7c0f3afa89439ae24
