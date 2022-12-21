# 1. Enum


**Open  Ports**

80/tcp
443/tcp
22/tcp

## 1.1 port 80
![[Pasted image 20221126184530.png]]
![[Pasted image 20221126184552.png]]


![[Pasted image 20221126193341.png]]
there is an extra header when connection to port 80 and leaks domain name 



## 1.2 https
![[Pasted image 20221126184614.png]]
OpenSSL/1.1.1k mod_fcgid/2.3.9


## 1.3 office.paper and subdomain enum

running wfuzz on office.paper 
![[Pasted image 20221126193745.png]]
found chat.office.paper
![[Pasted image 20221126193827.png]]

## 1.4 office.paper wordpress

![[Pasted image 20221126194607.png]]
going to wp-login
![[Pasted image 20221126195148.png]]
and looks like lost password feature is working
 so we can enumurate users

  ![[Pasted image 20221126195239.png]]
  ![[Pasted image 20221126195254.png]]
trying username we saw earlier and it exists
![[Pasted image 20221126195318.png]]
wpscan results 
![[Pasted image 20221126195456.png]]
found a lead on the comments 
![[Pasted image 20221126200903.png]]
using wpscan with API choose a couple of vulnerabilities but what stood out from them is a draft reading vuln ![[Pasted image 20221126202539.png]]
https://www.exploit-db.com/exploits/47690
![[Pasted image 20221126202620.png]]
 
![[Pasted image 20221126202635.png]]
it's the secret registration link to create account on rocket.chat
![[Pasted image 20221126202719.png]]
and we are in rocket.chat 

![[Pasted image 20221126202753.png]]

xdd

![[Pasted image 20221126203443.png]]
there is a bot on the server that can access the file system and grab files from it
![[Pasted image 20221126203515.png]]
![[Pasted image 20221126204432.png]]
inside ../hubot
![[Pasted image 20221126204731.png]]
![[Pasted image 20221126204748.png]]
looks like it's a git repo
and .env probably has interesting info 
![[Pasted image 20221126204818.png]]
or
inside /proc/self/enviorn 
# 3. privesc
dwight:Queenofblad3s!23
trying password we got with the user dwight cos we already know he exists and it works!
we are in 



![[Pasted image 20221126204957.png]]

![[Pasted image 20221126205110.png]]
running linpeas and trying exploits we found
polkit is running on the machine so let's start with that 
![[Pasted image 20221126230110.png]]
https://github.com/secnigma/CVE-2021-3560-Polkit-Privilege-Esclation
uploading the exploit to the target and running it  it's a time based exploit so it might take multiple tries to succeed 
![[Pasted image 20221126230206.png]]
![[Pasted image 20221126230217.png]]
and it worked!
now we can login with the user secnigma: secnigmaftw
![[Pasted image 20221126230245.png]]
![[Pasted image 20221126230304.png]]
![[Pasted image 20221126230514.png]]
fb940d9e463d94ca26b684094c25dd7f