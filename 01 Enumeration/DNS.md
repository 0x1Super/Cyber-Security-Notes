https://securitytrails.com/blog/dns-enumeration
host -t RECORD IP  - to show the record of the domain 
host -t ns zonetransfer.me 
host -l IP NS - sevrer that is vul 
host -l zonetransfer.me nsztml1.digi.ninja.
dig RECORD DOMAIN/IP 
dig axfr zonetransfer.me @nsztm1.digi.ninja.

# dig
```bash
dig ANY @<DNS_IP> <DOMAIN>     #Any information
dig A @<DNS_IP> <DOMAIN>       #Regular DNS request
dig AAAA @<DNS_IP> <DOMAIN>    #IPv6 DNS request
dig TXT @<DNS_IP> <DOMAIN>     #Information
dig MX @<DNS_IP> <DOMAIN>      #Emails related
dig NS @<DNS_IP> <DOMAIN>      #DNS that resolves that name
dig -x 192.168.0.2 @<DNS_IP>   #Reverse lookup
dig -x 2a00:1450:400c:c06::93 @<DNS_IP> #reverse IPv6 lookup

#Use [-p PORT]  or  -6 (to use ivp6 address of dns)
```
