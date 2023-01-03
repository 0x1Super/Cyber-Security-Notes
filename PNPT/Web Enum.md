# assetfinder
*Subdomain enum*
```bash
assetfinder DOMAIN # or implement it into bash script 

# flags
--subs-only # only domain subdomains
```


# amass 
*Subdomain enum*

```
amass enume -d DOMAIN
e.g.
amass enum -d tesla.com

# flags
-d #domain 

```

# httprobe 
*Subdomain alive enum*
adds http and https to text in file and checks if the website replies or not 
```
cat subdomain.txt | httprobe

cat subdomain.txt | httprobe -s -p  https:443 # (checks for https only)  
# remove https and ports for nmap scan 
cat subdomain.txt | httprobe -s -p  https:443 | sed 's/https\?:\/\///' | tr -d ':443'


# flags

-s # remove default ports 
-p # port 
```

# gowitness
*Taking screenshot of subdomains*
```
gowitness single URL 

#flags
single # for single screenshot 

```

https://pastebin.com/MhE6zXVt


# script to run multiple tools

```python
#!/bin/bash
url=$1 # taking user input

# create $url folder if it doesn't exist 
if [ ! -d "$url" ];then
       mkdir $url
fi

# create recon folder if it doesn't exist 
if [ ! -d "$url" ];then
       mkdir $url/recon
fi

#banner assetfinder
echo "[+] Harvesting subdomains with assetfinder...."  

assetfinder $url >> $url/recon/assets.txt # send the output to a file called assets.txt
cat $url/recon/assets.txt | grep $1 >> $url/recon/final.txt # get only subdomains
rm $url/recon/assets.txt

#banner Amass
#echo "[+] Harvesting subdomains with Amass...." 

#amass enum -d $url >> $url/recon/f.txt
#sort -u   $url/recon/f.txt >> $url/recon/final.txt
#rm $url/recon/f.txt

#banner httprobe
echo "[+] Probing for alive domains...."  

cat $url/recon/final.txt | sort -u | httprobe -s -p  https:443 | sed 's/https\?:\/\///' | tr -d ':443' >> $url/recon/alive.txt

```




---
Tags: #amass #assetfinder #OSINT #subdomains #httprobe #gowitness
Resources:
[assetfinder](https://github.com/tomnomnom/assetfinder)
[Amass](https://github.com/OWASP/Amass)
[httprobe](https://github.com/tomnomnom/httprobe)
[GoWitness](https://github.com/sensepost/gowitness)
[sumrecon](https://github.com/thatonetester/sumrecon)
[tcm_sumrecon](https://pastebin.com/MhE6zXVt)
[Bug_hunter's_Methodology](https://www.youtube.com/watch?v=uKWu6yhnhbQ)
[Nahamsec_recon_playlist](https://www.youtube.com/watch?v=uKWu6yhnhbQ)

