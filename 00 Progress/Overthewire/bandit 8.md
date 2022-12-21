ssh bandit1@bandit.labs.overthewire.org -p 2220 PW: cvX2JJa4CFALtqS87jk27qwqGhBM9plV
Task: The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

ANS:
ls
sort data.txt | uniq -u 
![[Pasted image 20211124134809.png]]
#PW : UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR