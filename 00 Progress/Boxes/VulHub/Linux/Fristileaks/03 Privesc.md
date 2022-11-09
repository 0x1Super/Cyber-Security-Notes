in www we found notes
![[Pasted image 20211028170336.png]]
running linpeas
![[Pasted image 20211028171112.png]]
![[Pasted image 20211028171417.png]]
well then we create that runthis file and also create a python reverse shell script and add /usr/bin/python like this 
![[Pasted image 20211028175324.png]]
![[Pasted image 20211028175345.png]]
and we get a shell back!
![[Pasted image 20211028175558.png]]
![[Pasted image 20211028180733.png]]
we found that there is a .py program which encrypt the strings and then encode them into base64 
trying to reverse engineer it
![[Pasted image 20211028184031.png]]
now let's try the values we got earlier in the 2 files 
![[Pasted image 20211028184049.png]]
![[Pasted image 20211028184119.png]]
the file was called whoisyourgod now
and there is a user called fristigod let's try to login as him with the passwords we just got 
![[Pasted image 20211028184210.png]]
we are in 
![[Pasted image 20211028184324.png]]
searching around we found the .secret directory and it has a doCom executable checking out the .bash_history of our user fristi god 
we found out that we can run commands as the user fristi with the doCom program 
![[Pasted image 20211028185932.png]]
![[Pasted image 20211028185944.png]]
then we did a bash reverse shell to our machine 
now we are fristi aka ROOT
![[Pasted image 20211028190021.png]]
#rootflag Y0u_kn0w_y0u_l0ve_fr1st1
