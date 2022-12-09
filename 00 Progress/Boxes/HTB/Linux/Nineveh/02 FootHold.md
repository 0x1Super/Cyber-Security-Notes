~amrois 
on the first admin page 
![[Pasted image 20210930190747.png]]
url looks like LFI vul so tried some methods didn't work 
on 2nd admin page phpliteadmin we found a #CVE for it [[CVEs]]
let's first create a database and call it the same as the txt file we found on the http admin web page  
but with .php extension and we put the code 
```
<?php phpinfo()?> 
```
as the value in the database table 
 to test if the code will be executed 
testing the database we created
![[Pasted image 20210930191126.png]]
and it works! 
now let's try to get a get code execution on the server with [[02 Exploitation/PHP]]
![[Pasted image 20210930192430.png]]
and it works! we have code execution on the server let's try reverse shell now
doing which python to locate if python is on the server
trying python3 and it's on the server 
using pentestmonkey reverse shells and picking up the python rev shell 
and we are in!
![[Pasted image 20210930192925.png]]
