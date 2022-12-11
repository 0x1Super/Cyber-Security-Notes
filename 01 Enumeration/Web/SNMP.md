# snmp

```bash
sudo apt install snmp-mibs-downloader

sudo vi /etc/snmp/snmp.conf  # comment MIBS

snmpwalk -c public -v2c $ip 

```




## Enumerate Community strings

```
./onesixtyone -c /usr/share/seclists/Discovery/SNMP/common-snmp-community-strings.txt $ip
```

```
python snmpbrute.py -t $ip
```

```
## snmp-check

snmp-check $ip -c public
```


## Nmap script 

```
nmap -sU -p161 --script "snmp-*" $ip

nmap -n -vv -sV -sU -Pn -p 161,162 –script=snmp-processes,snmp-netstat IP
```

## snmpwalk 




```
apt install snmp-mibs-downloader # translates MIBs into readable format
for community in public private manager; do snmpwalk -c $community -v1 $ip; done

snmpwalk -c public -v1 $ip

snmpenum $ip public windows.txt

snmpwalk -v 2c -c public 10.10.11.107

```

## snmpget

```bash
snmpget -v 1 -c public 192.168.2.46 .1.3.6.1.4.1.11.2.3.9.1.1.13.0 #Getting a JetDirect password
```


Less noisy

```
snmpwalk -c public -v1 $ip 1.3.6.1.4.1.77.1.2.25
```

Based on UDP, stateless and susceptible to UDP spoofing

```
nmap -sU --open -p 16110.1.1.1-254 -oG out.txt

snmpwalk -c public -v1 $ip # we need to know that there is a community called public

snmpwalk -c public -v1 $ip 1.3.6.1.4.1.77.1.2.25 # enumerate windows users

snmpwalk -c public -v1 $ip 1.3.6.1.2.1.25.4.2.1.2 # enumerates running processes

nmap -vv -sV -sU -Pn -p 161,162 --script=snmp-netstat,snmp-processes $ip
```

## SNMPv3 enumeration

```
wget https://raw.githubusercontent.com/raesene/TestingScripts/master/snmpv3enum.rb; ./snmpv3enum.rb
```

## Wordlist

/usr/share/metasploit-framework/data/wordlists/snmp_default_pass.txt

## SNMP MIB Trees

-   1.3.6.1.2.1.25.1.6.0 - System Processes
    

-   1.3.6.1.2.1.25.4.2.1.2 - Running Programs
    

-   1.3.6.1.2.1.25.4.2.1.4 - Processes Path
    

-   1.3.6.1.2.1.25.2.3.1.4 - Storage Units
    

-   1.3.6.1.2.1.25.6.3.1.2 - Software Name
    

-   1.3.6.1.4.1.77.1.2.25 - User Accounts
    

-   1.3.6.1.2.1.6.13.1.3 - TCP Local Ports
    

## Exploitation



-   Gather version numbers
    

-   Searchsploit
    

-   Default Creds
    

-   Creds previously gathered
    

-   Download the software