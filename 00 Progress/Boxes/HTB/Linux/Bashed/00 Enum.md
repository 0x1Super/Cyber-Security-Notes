# nmap scan 
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
in go 
# gobuster scan
/dev                  (Status: 301) [Size: 312] [--> http://10.129.226.61/dev/]                                                        
/js                   (Status: 301) [Size: 311] [--> http://10.129.226.61/js/]                                                         
/config.php           (Status: 200) [Size: 0]                                                                                          
/fonts                (Status: 301) [Size: 314] [--> http://10.129.226.61/fonts/]                                                      
/single.html          (Status: 200) [Size: 7477]                                      
/uploads 
gobuster scan we found /dev and it lead us to a directory which has phpbashed terminal and we got 

using a python reverse shell to get our stable shell rather than the web shell [[Python]]
## Second method

uploading php reverse shell to the server and access it through /uploads
![[Pasted image 20221111144925.png]]

then we can try to access the reverse shell through the website URL 
first we set up a listener 
![[Pasted image 20221111145121.png]]

and we got a shell


#userflag 2c281f318555dbc1b856957c7147bfc1

