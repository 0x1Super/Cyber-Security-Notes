ssh bandit1@bandit.labs.overthewire.org -p 2220 PW: [[bandit 13]]4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
Task: The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

ANS:
cat /etc/bandit_pass/bandit14
telnet localhost 30000
![[Pasted image 20211124144213.png]]
#PW : BfMYroe26WYalil77FoDi9qh59eK5xNr
