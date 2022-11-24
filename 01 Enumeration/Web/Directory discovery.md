
# Gobuster 


 
```
gobuster dir -u 10.10.10.181 -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

## Gobuster quick directory busting 



```
gobuster -u $ip -w /usr/share/seclists/Discovery/Web_Content/common.txt -t 80 -a Linux
```

## Gobuster directory busting 
```
wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/Top1000-RobotsDisallowed.txt; gobuster -u http://$ip -w Top1000-RobotsDisallowed.txt

gobuster dir -u http://$ip -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x php -o gobuster-root -t 50
```

## Gobuster comprehensive directory busting

```
gobuster -s 200,204,301,302,307,403 -u $ip -w /usr/share/seclists/Discovery/Web_Content/big.txt -t 80 -a 'Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0'
```

## Gobuster search with file extension

```
gobuster -u $ip -w /usr/share/seclists/Discovery/Web_Content/common.txt -t 80 -a Linux -x .txt,.php
```

## if stuck
```
for file in $(ls /usr/share/seclists/Discovery/Web-Content); do gobuster -u http://$ip/ -w /usr/share/seclists/Discovery/Web-Content/$file -e -k -l -s "200,204,301,302,307" -t 20 ; done
```
 **FIX STATUS CODE ERROR**
 ```
 -b "STATUS_CODE"
```

## vhost
```
gobuster vhost -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u http://thetoppers.htb --append-domain
```

# Wfuzz

```
wfuzz -c -z file,/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --sc 200 http://$ip/FUZZ
```
```
wfuzz -c -f subdomains.txt -w /usr/share/wordlists/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt -u "http://shoppy.htb/" -H "Host: FUZZ.shoppy.htb" --hl 7
```
# Erodir by PinkP4nther

```
./erodir -u http:/$ip -e /usr/share/wordlists/dirb/common.txt -t 20
```
# dirsearch.py

```
cd /root/dirsearch; python3 dirsearch.py -u http://$ip/ -e .php
```
