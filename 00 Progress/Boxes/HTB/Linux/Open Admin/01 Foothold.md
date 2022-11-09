looking into searchsploit we found a cve for version 18.1.1
![[Pasted image 20211211212958.png]]

download the exploit we found https://www.exploit-db.com/exploits/47691 #opennetadmin 
modifying the exploit to our target IP 

![[Pasted image 20211211214527.png]]

and we have RCE on the server 
![[Pasted image 20211211214548.png]]

now we need to get a shell on the server after trying a lot of reverse shells python one seemed to work
first we got the shell from [[Python]] then upload it to the server using python -m http.server and wget on the server
then we ran the rev shell using python3
![[Pasted image 20211212124315.png]]
now we have a shell on the box and we can start working 
