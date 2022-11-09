Tags:Relevant Box THM
When: [[todo 2]]

##Note:

# Enumeration 
First as usual we export IP=Machine_IP then start with Nmap scan : 
First we run nmap  $IP
80/tcp   open  http          Microsoft IIS httpd 10.0
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp open  ms-wbt-server Microsoft Terminal Services
49667/tcp
49663/tcp
49669/tcp
open ports then we run our normal nmap scan on the open ports for efficiency

nmap -sC -sV  -p 80,135,139,445,3389 -oA Nmap/Allscan.log $IP

SMB works on the machine 
using searchsploit we find out there is a CVE for that server for Remote code execution  on SMB 
==Microsoft Windows Server 2008 R2 (x64) - 'SrvOs2FeaToNt' SMB Remote Code Execut | windows_x86-64/remote/41987.py==

Let's first enumerate the smb server to see if it allows anonymus login 
using smbclient -L //$IP/ to list the directories 
	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	nt4wrksv        Disk      

smbclient -L //$IP/nt4wrksv we logged in without a password #Vul common smb vulnerability to login to directories without a password 
we found out a passwords.txt let's download it with get command 
[User Passwords - Encoded]
Qm9iIC0gIVBAJCRXMHJEITEyMw==
QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk

using Cyberchef https://gchq.github.io/CyberChef/ to decode base64 
we find creds
##creds 
passwords.txt:Bob:!P@$$W0rD!123
passwords.txt:Bill:Juw4nnaM4n420696969!$$$
we managed to connect to rpcclient -U bob $IP with his password 
well that didn't go as planned
we tried to poke around the website with the ports we seen and i did find out something interesting on port 49663/tcp that we can access nt4wrksv share and files from there 
we tried using Put command on smb server to upload files and it works! #vul
after some google-fu we found out that most of Microsoft IIS runs aspx and we tried to upload our aspx reverse shell and execute it through website 
we are in!
# Foothold 
#userflag: in Bob's Desktop : THM{fdk4ka34vk346ksxfr21tg789ktf45}

using whoami /priv 
SeAssignPrimaryTokenPrivilege              Disabled
SeIncreaseQuotaPrivilege                        Disabled
SeAuditPrivilege                                       Disabled
SeChangeNotifyPrivilege                         Enabled 
SeImpersonatePrivilege                           Enabled 
SeCreateGlobalPrivilege                          Enabled 
SeIncreaseWorkingSetPrivilege              Disabled

cos SeImpersonatePrivilege is enabled we can try PrintSpoofer tool to escalate we open a http server to host our tool on my machine and wget the tool from the machine  https://github.com/itm4n/PrintSpoofer
and we are root! #vul

#rootflag: THM{1fk5kf469devly1gl320zafgl345pv}



# Privesc 



# Conclusion 

- Learned about that IIS servers use most of the time use Aspx and running Aspx reverse shell
- accessing smb from browser from a port and uploading files 
- printspoofer tool to privesc 
- i have marked the issues with this machine with  hashtag vul 

Issues was 
- Weak smb Security 
- Allowed Update in smb from any user
- SeImpersonatePrivilege enabled which lead to privesc

----------------------------------------------------------------------------------
