# PHP Filters

## Input Filters 
>[PHP Filters](https://www.php.net/manual/en/filters.php) are a type of PHP wrappers,where we can pass different types of input and have it filtered by the filter we specify. To use PHP wrapper streams, we can use the `php://` scheme in our string, and we can access the PHP filter wrapper with `php://filter/`.

---

##  Fuzzing for PHP Files

>The first step would be to fuzz for different available PHP pages with a tool like `ffuf` , `gobuster` or `feroxbuster`, and because we have a LFI we don't need to worry about 301,302 or 403 status codes cos we can read it's content anyway using LFI

---
## Standard PHP Inclusion
>Trying to read `.php` source code using LFI might not work because the page will render the PHP inside the file we are trying to read that's why we can use PHP `wrappers` to convert the output of the `.php` file into for example base64

---
## Source Code Disclosure
>Once we have a list of potential PHP files we want to read, we can start disclosing their sources with the `base64` PHP filter. Let's try to read the source code of `config.php` using the base64 filter, by specifying `convert.base64-encode` for the `read` parameter and `config` for the `resource` parameter, as follows:

Code:
```url
php://filter/read=convert.base64-encode/resource=config
http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=config
```
>>**Note:** We intentionally left the resource file at the end of our string, as the `.php` extension is automatically appended to the end of our input string, which would make the resource we specified be `config.php`.

then we can use `base64 -d` to decode base64 and get source code
Code:
```bash
echo 'PD9waHAK...SNIP...KICB9Ciov' |base64 -d
```

---
## Exercise 
**Fuzz the web application for other php scripts, and then read one of the configuration files and submit the database password as the answer**

extracting index.php to test our lfi
![[Pasted image 20230305172426.png]]
![[Pasted image 20230305172513.png]]
![[Pasted image 20230305172517.png]]
`gobuster dir -u http://142.93.37.0:30120 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x php`
![[Pasted image 20230305172745.png]]
`php://filter/read=convert.base64-encode/resource=configure `
![[Pasted image 20230305172820.png]]

![[Pasted image 20230305172919.png]]
![[Pasted image 20230305172937.png]]


