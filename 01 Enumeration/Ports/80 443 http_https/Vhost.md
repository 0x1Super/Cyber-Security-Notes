

## vhost
```
gobuster vhost -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u http://thetoppers.htb --append-domain
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
