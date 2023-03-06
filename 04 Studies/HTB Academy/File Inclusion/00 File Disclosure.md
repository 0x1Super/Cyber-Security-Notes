LFI and RFI attack range from sensitive information disclosure and Cross-site Scripting to Remote Code Execution
LFI we can  view or access the Local files on the server
in LFI we can access files on system like 
Example - http://example.com/?file=filename.php
or in worst case scenario upload a malicious   code to the server 	http://example.com/?file=../../uploads/evil.php
but even if we can upload we still don't know if it will be uploaded on same server that has the LFI exploit 

# Path Traversal

##  Filename Prefix
Code: 
```php 
include("lang_" . $_GET['language']);
```

in this case using ../../ won't work because the output will be like this lang_../../
so what we can do is adding / first 
eg.
`language=/../../../etc/passwd`

---

## Appended Extensions

Code:
```php
include($_GET['language'] . ".php");
```

in this case the code will append .php to the file name provided so using /etc/passwd will be /etc/passwd.php which does not exist

---

##  Second-Order Attacks
As we can see, LFI attacks can come in different shapes. Another common, and a little bit more advanced, LFI attack is a `Second Order Attack`. This occurs because many web application functionalities may be insecurely pulling files from the back-end server based on user-controlled parameters.

For example, a web application may allow us to download our avatar through a URL like (`/profile/$username/avatar.png`). If we craft a malicious LFI username (e.g. `../../../etc/passwd`), then it may be possible to change the file being pulled to another local file on the server and grab it instead of our avatar.

In this case, we would be poisoning a database entry with a malicious LFI payload in our username. Then, another web application functionality would utilize this poisoned entry to perform our attack (i.e. download our avatar based on username value). This is why this attack is called a `Second-Order` attack.

Developers often overlook these vulnerabilities, as they may protect against direct user input (e.g. from a `?page` parameter), but they may trust values pulled from their database, like our username in this case. If we managed to poison our username during our registration, then the attack would be possible.

Exploiting LFI vulnerabilities using second-order attacks is similar to what we have discussed in this section. The only variance is that we need to spot a function that pulls a file based on a value we indirectly control and then try to control that value to exploit the vulnerability.

---

## Exercise
![[Pasted image 20230304170200.png]]
![[Pasted image 20230304170220.png]]
![[Pasted image 20230304170318.png]]

### Username starts with b:
trying path traversal using ../ 
`188.166.150.33:32607/index.php?language=../../../../etc/passwd`
![[Pasted image 20230304170406.png]]
user is barry 

### Flag.txt

![[Pasted image 20230304170613.png]]




