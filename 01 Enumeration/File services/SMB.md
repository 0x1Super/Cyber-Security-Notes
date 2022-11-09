# enumerating SMB 
smbclient -L //$IP/ ##to check for directories without logging in 
smbclient //$IP/Directory login without auth

Enumerate Hostname : nmblookup -A [ip] 
## List Shares
smbmap -H [ip/hostname]
echo exit | smbclient -L \\\\[ip]
nmap --script smb-enum-shares -p 139,445 [ip]
- Check Null Sessions
smbmap -H [ip/hostname]
smbclient \\\\[ip]\\[share name]
Check for Vulnerabilities - nmap --script smb-vuln* -p 139,445 [ip]
Overall Scan - enum4linux -a [ip]
Manual Inspection
smbver.sh [IP] (port) [Samba]
check pcap


# SMB server
smbserver.py NAME $(DIR) -smb2support -user USER -password password
exde1234


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