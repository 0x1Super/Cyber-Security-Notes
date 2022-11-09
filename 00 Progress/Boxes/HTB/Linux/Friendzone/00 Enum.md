starting with port scan on the target
# port scan 

using nmap
21/tcp    open     ftp          syn-ack 

22/tcp  open   ssh         syn-ack      OpenSSH 7.6p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
53/tcp  open   domain      syn-ack      ISC BIND 9.11.3-1ubuntu1.2 (Ubuntu Linux)
80/tcp  open   http        syn-ack      Apache httpd 2.4.29 ((Ubuntu))
139/tcp open   netbios-ssn syn-ack      Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
212/tcp closed anet        conn-refused 
443/tcp open   ssl/http    syn-ack      Apache httpd 2.4.29 ((Ubuntu))
445/tcp open   netbios-ssn syn-ack      Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)






port 80 enum

![[Pasted image 20211218144600.png]]
info@friendzoneportal.red



port 53 






port 445 smb(samba)
![[Pasted image 20211218150844.png]]
![[Pasted image 20211218150901.png]]
found creds.txt in smb directory general
![[Pasted image 20211218151056.png]]
admin:WORKWORKHhallelujah@#                                                                                                                                   
