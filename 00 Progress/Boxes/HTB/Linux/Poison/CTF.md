
Open ports


![[Pasted image 20221129192500.png]]

![[Pasted image 20221129192524.png]]
found LFI
![[Pasted image 20221129193326.png]]
![[Pasted image 20221129193337.png]]
![[Pasted image 20221129193509.png]]

charix:Charix!2#4%6&8(0
```
[|Ֆz!
```
![[Pasted image 20221129193624.png]]
ssh login successful 
/var/mail/charix
 

found secret.zip file and after uploading it to our machine
unzipping it with same password as charix user
it contains weird chars 
![[Pasted image 20221129213257.png]]
running linpeas on the target
found that root is running vnc 
![[Pasted image 20221129213455.png]]
![[Pasted image 20221129213504.png]]

let's ssh tunnel these ports to our machine
![[Pasted image 20221129213522.png]]
![[Pasted image 20221129213742.png]]
![[Pasted image 20221129213635.png]]
because we know that this is vnc so let's connect to it 
trying charix password and didn't work
vnc can use password files so prob the secret file we found earlier is vnc password
![[Pasted image 20221129213830.png]]
![[Pasted image 20221129214052.png]]
and we are root
decrypt vnc password
https://github.com/jeroennijhof/vncpwd
