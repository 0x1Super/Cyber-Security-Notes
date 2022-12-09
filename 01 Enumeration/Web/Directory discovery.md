
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
 -a user-agent change
 --exclude-length # exclude length
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

```

## Wfuzz for subdomains

```bash

wfuzz -c -f subdomains.txt -w /usr/share/wordlists/Discovery/DNS/subdomains-top1million-20000.txt -u "http://nunchucks.htb/" -H "Host:FUZZ.nunchucks.htb" --hl 7

--hl # filter wordsize all subdomains giving 200  status
--hw 
```


## Valid subdomains 

```bash
# To remove results with a specific word count, you can append your command w/ `--hw <value>`. For example, our new command that removes results that respond w/ a word count of 290 would look like the following:

wfuzz -c -f sub-fighter -w top5000.txt -u 'http://target.tld' -H "Host: FUZZ.target.tld" --hw 290
```

# ffuf
```bash
# Vhost scan

ffuf -u http://trick.htb -w /usr/share/wordlists/Discovery/DNS/subdomains-top1million-5000.txt -H 'Host: FUZZ.trick.htb' -fs 5480# filter size



# injection fuzzing



fuff -u http://script.htb -d 'ip=127.0.0.1FUZZ&action=scan' -w /usr/share/wordlists/seclists/Fuzzing/special-chars.txt 






-H # Header
-x # Proxy 
-d # data POST 
-mc all # all status codes


```


# Erodir by PinkP4nther

```
./erodir -u http:/$ip -e /usr/share/wordlists/dirb/common.txt -t 20
```
# dirsearch.py

```
cd /root/dirsearch; python3 dirsearch.py -u http://$ip/ -e .php
```
