ssh bandit1@bandit.labs.overthewire.org -p 2220 PW: cvX2JJa4CFALtqS87jk27qwqGhBM9plV
Task: The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

ANS:
ls
strings data.txt | grep "="
![[Pasted image 20211124135027.png]]
#PW : truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk