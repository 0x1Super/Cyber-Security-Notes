Open Ports
22/tcp
3000/tcp Node.js
![[Pasted image 20221129053057.png]]
directory brute force is not working correctly so using burp is our best option 
![[Pasted image 20221129053204.png]]
![[Pasted image 20221129053226.png]]

http://node.htb:3000/assets/js/app/app.js
http://node.htb:3000/api/users/
![[Pasted image 20221129055822.png]]
![[Pasted image 20221129054450.png]]
tom:spongebob
mark:snowflake
myP14ceAdm1nAcc0uNT:manchester
download backup file
![[Pasted image 20221129063823.png]]
convert it from base64 
```bash
cat myplace.backup | base64 -d > unkown
file unkown
```
![[Pasted image 20221129063933.png]]
```bash
unzip unkown.zip
```
![[Pasted image 20221129063948.png]]
```bash
zip2john unkown.zip > hash
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```
![[Pasted image 20221129064039.png]]
![[Pasted image 20221129064054.png]]
we have the website backup 
![[Pasted image 20221129064122.png]]
mark:5AYRft73VtFpc84k
trying ssh
and we are in as mark
![[Pasted image 20221129064203.png]]
