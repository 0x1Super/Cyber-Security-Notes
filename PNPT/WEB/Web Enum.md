# assetfinder
*Subdomain enum*
```
assetfinder DOMAIN or implement it into bash script 


#!/bin/bash
url=$1

if [ ! -d "$url" ];then
       mkdir -p $url/recon
fi

echo "[+] Harvesting subdomains with assetfinder...." 
assetfinder $url >> $url/recon/assets.txt
cat $url/recon/assets.txt | grep $1 >> $url/recon/final.txt
rm $url/recon/assets.txt


```
# amass 
*Subdomain enum*
https://github.com/OWASP/Amass/blob/master/doc/install.md

```
amass enume -d (domain) DOMAIN
e.g.
amass enum -d tesla.com

```

# httprobe 
*Subdomain alive enum*
adds http and https to text in file and checks if the website replies or not 
```
cat subdomain.txt | httprobe -s -p  https:443 (checks for https only)  
for nmap format we add sed
cat subdomain.txt | httprobe -s -p  https:443 | sed 's/https\?:\/\///' | tr -d ':443'
```

# gowitness
*Taking screenshot of subdomains*
```
gowitness single (for single screenshot) URL
```

# Automating all

https://github.com/thatonetester/sumrecon
https://pastebin.com/MhE6zXVt
