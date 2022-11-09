IP = 192.168.1.8
adding kioptrix3.com to /etc/hosts file 

# Port scan 
full port scan
22/tcp open  ssh     syn-ack
80/tcp open  http    syn-ack
script/version scan 
80/tcp open  http    syn-ack Apache httpd 2.2.8 ((Ubuntu) PHP/5.2.4-2ubuntu5.6 with Suhosin-Patch)

22/tcp open  ssh     syn-ack OpenSSH 4.7p1 Debian 8ubuntu1.2 (protocol 2.0)                                                                                   


  
  
# gobuster
 
 /modules              (Status: 301) [Size: 355] [--> http://kioptrix3.com/modules/]
/cache                (Status: 301) [Size: 353] [--> http://kioptrix3.com/cache/]  
/data                 (Status: 403) [Size: 324]                                    
/gallery              (Status: 301) [Size: 355] [--> http://kioptrix3.com/gallery/]
/style                (Status: 301) [Size: 353] [--> http://kioptrix3.com/style/]  
/core                 (Status: 301) [Size: 352] [--> http://kioptrix3.com/core/]   
/phpmyadmin           (Status: 301) [Size: 358] [--> http://kioptrix3.com/phpmyadmin/]
running go buster again on 
kioptrix3.com/gallery 




# Port 80 http
checking the web site and poking around 
![[Pasted image 20211206154458.png]]
![[Pasted image 20211206154511.png]]
![[Pasted image 20211206155013.png]]
![[Pasted image 20211206154523.png]]
 in 
Blog 
![[Pasted image 20211206155306.png]]
![[Pasted image 20211206155502.png]]
we can type in here comments 

Login

Found login page 
![[Pasted image 20211206155058.png]]
  
there is /phpmyadmin login page also found from gobuster

we found some interesting directories like /version
![[Pasted image 20211206162317.png]]
![[Pasted image 20211206162340.png]]
we found that the website uses Gallarific 2.1
![[Pasted image 20211206163413.png]]

trying to change the sorting and we get an interesting URL

![[Pasted image 20211206163532.png]]

trying changing the url to some sql syntax and we get an error
![[Pasted image 20211206163741.png]]

that confirms that website id parameter  is vulnerable to sql injection and from the error we got that the server is running mySQL 

using [[SQL-Injection (SQLi)]]

we get
dreg:0d3eccfb887aabd50f243b3f155c0f85
loneferret:5badcaf789d3d1d09794d8f021f40f0e
2 users and their hashes

now we can try to crack them with hashcat 
hashcat -m 0 -a 0 hash1 /usr/share/wordlists/rockyou.txt 

![[Pasted image 20211206191713.png]]

dreg:Mast3r
![[Pasted image 20211206191757.png]]
loneferret:starwars 
we try the creds we found on ssh and we are in!
![[Pasted image 20211206192102.png]]
