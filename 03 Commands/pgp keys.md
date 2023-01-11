decrypt pgp file using .asc key

Import the key:

```
gpg --import key.asc
```

Decrypt the file:

```
gpg --decrypt file.pgp
```

# bruteforce Decrypt key passphrase 

```
gpg2john file.asc > hash
john hash --wordlist=/usr/share/wordlist/rockyou.txt
```
---
Tags: #john #pgp 
