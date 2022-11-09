running -l we get 
![[Pasted image 20210928190819.png]]
and in the script 
![[Pasted image 20210928190840.png]]
python import the module random 
and python looks first in the current directory for the modules as Python Module Manipulation so we can create a random.py script to give us a shell 
import os 
os.system("/bin/bash -p")
and now we are rabbit user
there is a SUID elf program on rabbit dir 
![[Pasted image 20210928192311.png]]
running strings on the program we find out that it uses date command 
we can try to do PATH poison and create a script called date to run instead of the actual date command to give us shell
date script /bin/bash -p
![[Pasted image 20210928192610.png]]
in hatter dir we found password.txt which has hatters password
sshing as hatter 
then we found that perl can create suid files 
checking GTFO ![[Pasted image 20210928194338.png]]
running that command and we are root
#userflag thm{"Curiouser and curiouser!"}
#rootflag thm{Twinkle, twinkle, little bat! How I wonder what you're at!}

