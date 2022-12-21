ssh bandit1@bandit.labs.overthewire.org -p 2220 PW: 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
Task: The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on

ANS:
ls
ssh bandit14@localhost -i sshkey.private
![[Pasted image 20211124143943.png]]
![[Pasted image 20211124144043.png]]
#PW : 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e