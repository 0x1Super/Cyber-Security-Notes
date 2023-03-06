# Enumeration 

## open ports
```
PORT   STATE    SERVICE                                                                                       
21/tcp filtered ftp                                                                                           
22/tcp open     ssh                                                                                           
80/tcp open     http 
```
### Port 80
![[Pasted image 20230301220121.png]]
adding forge.htb to hosts
`echo '10.129.235.92 forge.htb' | sudo tee -a /etc/hosts`
running feroxbuster
`feroxbuster -u http://forge.htb -w /opt/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt `
![[Pasted image 20230301221905.png]]


![[Pasted image 20230301220301.png]]
![[Pasted image 20230301220319.png]]
Putting my own ip and we get a call back
![[Pasted image 20230301220403.png]]
creating a rev shell and try to upload it to the server
![[Pasted image 20230301220745.png]]

![[Pasted image 20230301224327.png]]
checking website looks like text is still intact 
open netcat listener 
![[Pasted image 20230301224632.png]]
the agent is python 
fuzzing for subdomain
`ffuf -u http://forge.htb -w /usr/share/wordlists/Discovery/DNS/subdomains-top1million-5000.txt -H 'Host: FUZZ.forge.htb' -fw 18`
![[Pasted image 20230301225320.png]]
![[Pasted image 20230301225402.png]]

# Foothold 

changing the admin subdomain to upper and lowercase chars in our SSRF
![[Pasted image 20230301232453.png]]
![[Pasted image 20230301232505.png]]
we got the results
trying both directories we found
![[Pasted image 20230301232654.png]]
and inside announcements we found creds
![[Pasted image 20230301232724.png]]
![[Pasted image 20230301233552.png]]
![[Pasted image 20230301233609.png]]
looks like it's the user home directory
accessing .ssh folder to try to grab id_rsa key and we have it
![[Pasted image 20230301233918.png]]
![[Pasted image 20230301233927.png]]
![[Pasted image 20230301234023.png]]
using id_rsa we found we are as user
![[Pasted image 20230301234106.png]]


# Privesc 
## user -> Root

![[Pasted image 20230301234217.png]]
Escaping pdb 
![[Pasted image 20230302000726.png]]



# Loot

ftp
	username: user
	password: `heightofsecurity123!`


---
Tags: #ctf #SSRF #sudo #vuln_code 
Resources:  [Forge](https://app.hackthebox.com/machines/376)