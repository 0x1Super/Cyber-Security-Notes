ftp $IP username anonymous password NULL



```bash
# Download everything


wget -r ftp://user:pass@server.com/
wget -m --no-passive ftp://anonymous:anonymous@10.10.10.10
```


```bash
# Bruteforce with nmap 
--script ftp-brute --script-args -userdb=USERS_FILE -p 21

```