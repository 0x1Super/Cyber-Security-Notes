#Chkrootkit 
in /var/www/ssl/secure_notes there is a png file which caught my attention cos of it's size running strings on the pic we get RSA private key ehh doesn't seem to work for now 
let's run linpeas
![[Pasted image 20210930200019.png]]
WELL after a while and reading linpeas 
![[Pasted image 20210930201054.png]]
the server is listening on port 22 but it doesn't seem to work when i try to connect 
so i tried something else what about connecting to ssh from the current www user and ssh to localhost
andd we are in!
![[Pasted image 20210930201147.png]]
#userflag 7e9909f64c83f126b5b1d7d891e97167
let's run linpeas again
well nothing much but checking the / directory again there is a directory called /report
checking that we find 
![[Pasted image 20210930205214.png]]
there is a file is created every min probably by cronjob 
checking the files and it's a 
chkrootkit 
which checks for system vul every min 
looking for vuln with searchsploit we find one [[CVEs]]
creating the /tmp/update file and putting reverse shell inside and we just wait a min to see if it will work 

![[Pasted image 20210930205900.png]]
we are root!
#rootflag 5f24a50da54d6f9698d42e176bed248c