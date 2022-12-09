ftp $IP username anonymous password NULL



```bash
# Download everything


wget -r ftp://user:pass@server.com/
```


```bash
# Bruteforce with nmap 
--script ftp-brute --script-args -userdb=USERS_FILE -p 21

```