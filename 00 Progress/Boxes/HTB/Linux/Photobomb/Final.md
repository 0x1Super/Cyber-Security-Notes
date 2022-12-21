
# **Nmap scan**

80/tcp nginx/1.18.0 (Ubuntu)
22/tcp


![[Pasted image 20221111152959.png]]

![[Pasted image 20221111153056.png]]
leads to login prompt
checking page source code
![[Pasted image 20221111153722.png]]
looks like we found a hidden message in the .js file
![[Pasted image 20221111153739.png]]
![[Pasted image 20221111153907.png]]
and we are in as pH0t0

found that 404 request gives this url
backend might be supporting python


![[Pasted image 20221111154852.png]]



opening up burp suite to investigate more back at /printer page tried to download a picture and found something interesting 
that the website uses POST method to download files 
![[Pasted image 20221111175107.png]]

trying to add ; after each parameter to see if any of them are injectable
found that **filetype** is injectable 

![[Pasted image 20221111175922.png]]


injecting a python reverse shell and encoding it through burp and setting up a listener
![[Pasted image 20221111175658.png]]
we are in as the user **wizard**
#userflag 7f9da8485ffe27e050498cd4ceee60e9
![[Pasted image 20221111180004.png]]
running sudo -l 
![[Pasted image 20221111191308.png]]

![[Pasted image 20221111191719.png]]
find command has no absolute path and we can leverage that to gain root access 
copying bash to a file called find 
echo /bin/bash > find 
![[Pasted image 20221111191854.png]]
now all we need to do is change the PATH to our current directory and run the script as root
 sudo PATH=$PWD: $PATH /opt/cleanup.sh 
![[Pasted image 20221111192104.png]]
#rootflag 1e6603cb1b3475e00ba97b5d3b684c6b