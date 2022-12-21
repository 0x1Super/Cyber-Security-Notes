ssh bandit5@bandit.labs.overthewire.org -p 2220 PW: koReBOKuIDDepwhWk7jZC0RTdopnAYKh
Task: The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

    human-readable
    1033 bytes in size
    not executable


ANS:
ls 
cd inhere/
ls -l
find -type f -size 1033c \! -executable -exec cat {} \;
![[Pasted image 20211124133723.png]]
#PW :DXjZPULLxYr17uwoI01bNLQbtFemEgo7