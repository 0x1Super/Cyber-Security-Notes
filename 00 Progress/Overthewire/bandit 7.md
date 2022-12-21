ssh bandit1@bandit.labs.overthewire.org -p 2220 PW: HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
Task: The password for the next level is stored in the file data.txt next to the word millionth

ANS:
ls
strings data.txt | grep millionth
![[Pasted image 20211124134431.png]]
#PW : cvX2JJa4CFALtqS87jk27qwqGhBM9plV