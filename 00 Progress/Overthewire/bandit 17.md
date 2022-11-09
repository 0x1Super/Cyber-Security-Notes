ssh bandit1@bandit.labs.overthewire.org -p 2220 PW: [[bandit 17]]
Task: There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

ANS:
ls
diff passwords.new passwords.old
![[Pasted image 20211124145854.png]]
#PW : kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd