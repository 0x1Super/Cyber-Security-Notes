```python
#!/bin/python3
import os
import sys
import socket
from datetime import datetime
#Define our target

os.system('mkdir nmap')
os.system('mkdir gobuster')
if len(sys.argv) == 2:
	# target = socket.gethostbyname(sys.argv[1]) # Translate name to IPV4
	target = sys.argv[1]
else:
	print("Invalid IP/URL")
	print("Syntax: python3 tool.py <IP>")
print(target)

# Banner

print("-" *50)
print("Nmap scanning......")
print("Time Started: "+str(datetime.now()))
print("-" *50)

# adding variables for ports

https = "443/tcp open"
http = "80/tcp open"
ftp = "21/tcp open"
smb = "445/tcp open"
redis = "63379/tcp open"
sql = "3306/tcp open"
sql2 = "1433/tcp open"
dns = "53/tcp open"
ssh = "22/tcp open"

# Nmap scan

def scanner():
	value = os.system('nmap ' + str(target) + ' -sC -sV -vv -oA nmap/scan >/dev/null')
	return value
nmap_final = scanner()
print(nmap_final)

# Creating file for open ports

os.system("cat nmap/scan.nmap | grep open > nmap/ports.txt")
os.system("cut nmap/ports.txt -d ' ' -f 1,2 > nmap/ports2.txt")

# savine file to variable for open ports

open_file = open('nmap/ports2.txt', 'r')
ports_file = open_file.read() 

# Open ports

if http in ports_file:
	print("http open")

print("\n")

if https in ports_file:
	print("https open")
	
print("\n")

if sql in ports_file:
	print("sql open")

print("\n")

if sql2 in ports_file:
	print("sql open")

print("\n")

if dns in ports_file:
	print("dns open")

print("\n")

if ftp in ports_file:
	print("ftp open")

print("\n")

if smb in ports_file:
	print("smb open")

print("\n")

if redis in ports_file:
	print("redis open")
	
print("\n")

#Directory Enum
#run gobuster if port 80 is open
if http in ports_file:
	print("-" *50)
	print("Scanning port 80 running gobuster")
	print("-" *50)
	print(os.system('gobuster dir -u '+target+' -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt -o gobuster/dir_brute.txt')) # >/dev/null 2>&1 &'))
# HTTPS scan 

if https in ports_file:
	print("-" *50)
	print("Scanning port 443 running gobuster")
	print("-" *50)
	print(os.system('gobuster dir -u '+target+':443 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -k -x php,txt -o gobuster/dir_brute.txt')) # >/dev/null 2>&1 &'))

#SMB enum

if smb in ports_file:
	print("-" *50)
	print("Scanning 445 SMB using smbclient")
	os.system('smbclient -L //'+target+'/ > smb_result.txt')
	print("-" *50)
	
	
if ftp in ports_file:
	print("-" *50)
	print("Scanning port 21 ftp rnning ftp")
	os.system('echo -ne ''\n''echo -ne ''\n''ftp '+target+' > ftp.txt')
	print("-" *50)
```