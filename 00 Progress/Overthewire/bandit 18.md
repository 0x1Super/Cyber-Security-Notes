ssh bandit1@bandit.labs.overthewire.org -p 2220 PW: kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
task: The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

ANS: ssh bandit1@bandit.labs.overthewire.org -p 2220  kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd /bin/sh
cat readme
#PW : IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x

