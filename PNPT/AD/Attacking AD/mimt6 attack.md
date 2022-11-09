# IPv6 poisoning 
If an attacker is on the local network, either physically (via a drop device) or via an infected workstation, it is possible to perform a DNS takeover using mitm6, provided IPv6 is not already in use in the network. When this attack is performed, it is also possible to make computer accounts and users authenticate to us over HTTP by spoofing the WPAD location and requesting authentication to use our rogue proxy. This attack is described in detail in my blog post on this subject from last year.

We can relay this NTLM authentication to LDAP (unless mitigations are applied) with ntlmrelayx and authenticate as the victim computer account. Computer accounts can modify some of their own properties via LDAP, which includes the msDS-AllowedToActOnBehalfOfOtherIdentity attribute. This attribute controls which users can authenticate to the computer as almost any account in AD via impersonation using Kerberos. This concept is called Resource-Based constrained delegation, and is described in detail by Elad Shamir and harmj0y. Because of this, when we relay the computer account, we can modify the account in Active Directory and give ourselves permission to impersonate users on that computer. We can then connect to the computer with a high-privilege user and execute code, dump hashes, etc. The beauty of this attack is that it works by default and does not require any AD credentials to perform.
## Requirements 
- ldaps running
- IPv6 enabled but not configured 


### Mitigation 
- IPv6 poisoning abuses the fact that Windows queries for an IPv6 address even in IPv4-only environments,  If you don't use IPv6 internally, the safest way to prevent mitm6 is to block DHCPv6 traffic incoming router advertisements in Windows Firewall via Group Policy. Disabling IPv6 entirely may have unwanted side effects.                                                                                                                                                                        
    1. Setting the following predefined rules to Block instead of Allow prevents the attack from working :
			a.       (Inbound) Core Networking - Dynamic Host Configuration Protocol for IPv6(DHCPV6-In)
			b.       (Inbound) Core Networking - Router Advertisement (ICMPv6-In)
			c>      (Inbound) Core Networking - Dynamic Host Configuration Protocol for IPv6(DHCPV6-Out)

2. If WPAD is not in use internally disable it via Group Policy and by disabling the WinHttpAutoProxySvc service.


3. Relaying to LDAP and LDAPS can only be mitigated by enabling both LDAP signing and LDAP channel binding. 


4. Consider Administrative users to the Protected Users group or marking them as Account is sensitive and cannot be delegated, which will prevent any impersonation of that user via delegation. 