ssh bandit1@bandit.labs.overthewire.org -p 2220 PW: BfMYroe26WYalil77FoDi9qh59eK5xNr
Task: The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

ANS:
openssl s_client -connect localhost:30001
BfMYroe26WYalil77FoDi9qh59eK5xNr
#PW : cluFn7wTiGryunymYOu4RcffSxQluehd