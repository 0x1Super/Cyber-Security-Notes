ssh bandit1@bandit.labs.overthewire.org -p 2220 PW: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
task:There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

ANS: 
first let's make a listener that sends the current level password to whoever connects to me
echo "GbKksEFF4yrVs6il55v6gwY5aVje5f0j" | nc -lvnp 50000 
then open second pane and use the
./suconnect 50000
![[Pasted image 20211124152009.png]]
#PW : gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
