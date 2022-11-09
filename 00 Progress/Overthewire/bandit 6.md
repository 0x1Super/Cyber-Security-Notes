ssh bandit1@bandit.labs.overthewire.org -p 2220 PW: DXjZPULLxYr17uwoI01bNLQbtFemEgo7
Task: The password for the next level is stored somewhere on the server and has all of the following properties:

    owned by user bandit7
    owned by group bandit6
    33 bytes in size


ANS:
ls
find / -user bandit7 -group bandit6 -size 33c -exec cat {} \; 2>/dev/null
![[Pasted image 20211124134232.png]]
#PW : HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs