#userflag = THM{int3rna1_fl4g_1}


now we find a txt file says : Internal Jenkins service is running on 172.17.0.2:8080
so let's try to do a [[Commands]]
and we are in there is a http server running jenkins 
running nmap on local host 127.0.0.1 and specifying port 8888

 8888/tcp open  http    Jetty 9.4.30.v20200611
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Jetty(9.4.30.v20200611)
_http-title: Site doesn't have a title (text/html;charset=utf-8).

we kinda got nothing from the search but we can try to brute force the jenkis and using default admin username admin 
hydra -l admin -P /usr/share/wordlists/rockyou.txt 127.0.0.1 -s 8888 http-post-form "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in:Invalid username or password"
and we let it roll
after getting shit ton of wrong passwords and wasting too much time with hydra cos it's shit tool we used OWASP zap attack mode to crack the password with fuzzing tool 
and we got [[00 Progress/Boxes/THM/Internal/04 Creds]]!! 

then we went to Script tool in jenkins and excuted a [[Java_rev]]
in /opt there is a note.txt
cat note.txt
Aubreanna,

Will wanted these credentials secured behind the Jenkins container since we have several layers of defense here.  Use them if you 
need access to the root user account.

root:tr0ub13guM!@#123
now ssh to the machine and we are root!
#rootflag : THM{d0ck3r_d3str0y3r}
[[00 Progress/Boxes/THM/Internal/05 Conclusion]]

