First we start with our nmap scan 

# nmap 
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP) [[SMTP 25]]
21/tcp  open  ftp         vsftpd 2.3.4    [[01 Enumeration/File services/Ftp]]


#[[01 Enumeration/File services/Ftp]]
trying anonymous login in ftp 
ftp $IP
Connected to 10.129.225.129.
220 (vsFTPd 2.3.4)
Name (10.129.225.129:kali): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 
we are in 
nothing in there let's try smb now 
#[[SMTP 25]]
we found a known SMB #CVE in metasploit and just ran the exploit and we are root? ex de?
#rootflag 1577d73d5c1a40cecb6aa076e421ec52
#userflag 69081129fad6a29087120a4ec1a1cf53




