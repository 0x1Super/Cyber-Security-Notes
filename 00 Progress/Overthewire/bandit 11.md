ssh bandit1@bandit.labs.overthewire.org -p 2220 PW: IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
Task:  The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

ANS:
ls
echo 'Gur cnffjbeq vf 5Gr8L4qetPEsPk8htqjhRK8XSP6x2RHh' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
![[Pasted image 20211124135434.png]]
#PW :5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu