upgrade shell 
[[Stable shells]]
looking inside djmardov directory we found that we have access to .backup file 
![[Pasted image 20211027135419.png]]
Super elite steg backup pw
UPupDOWNdownLRlrBAbaSSss
looking to the picture we found on the website earlier 
![[Pasted image 20211027135547.png]]
using password we found
![[Pasted image 20211027135607.png]]
Kab6h+m+bbp2J:HG
trying this password for the user djmardov and it worked
#userflag 4a66a78b12dc0e661a59d3f5c0267a8e
time to run linpeas
found unusual suid file
![[Pasted image 20211027142301.png]]
let's try to download the file to our machine and examine it 
![[Pasted image 20211027142522.png]]
the suid calls for ![[Pasted image 20211027142544.png]]
so can can edit the file and get a root shell cos of the suid bit 
![[Pasted image 20211027142747.png]]
and we are root 
#rootflag 8d8e9e8be64654b6dccc3bff4522daf3

