# Enumeration 

## open ports
```
22/tcp open  ssh?                                                                                             
|_ssh-hostkey: ERROR: Script execution failed (use -d to debug)                                               
| fingerprint-strings:                                                                                        
|   GenericLines:                                                                                             
|_    5d_f                                                                                                    
80/tcp open  http    nginx 1.19.6
|_http-trane-info: Problem with XML parsing of /evox/about
|_http-title: 503 Service Temporarily Unavailable
|_http-server-header: nginx/1.19.6
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port22-TCP:V=7.93%I=7%D=3/2%Time=6400F3A2%P=x86_64-pc-linux-gnu%r(Gener
SF:icLines,6,"5d_f\r\n");

```
![[Pasted image 20230302211647.png]]
![[Pasted image 20230302220342.png]]
![[Pasted image 20230302220349.png]]
we got call back
![[Pasted image 20230302220447.png]]

finding wellknown endpoints for api
![[Pasted image 20230302222443.png]]
![[Pasted image 20230302222517.png]]


# Foothold 



# Privesc 
# Loot



---
Tags: #ctf
Resources: 