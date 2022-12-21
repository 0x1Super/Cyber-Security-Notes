Important cURL commands
---
Using cURL without any arguments makes it send a GET request 
and we can provide creds with -u
for example 
curl -u admin:password  http://inlanefreight.com/ -vvv
or we can do it like this 

curl http://admin:password@inlanefreight.com/ -vvv
we can use -L to follow the redirection cos Curl doesn't do it by default 

______
Post Method
---
Curl -d to send a **POST** Request 
Let's try to login to the page from the POST section using `cURL`. The default "`Content-Type`" used by `cURL` is "`application/x-www-form-urlencoded`", the data for which can be passed using the "`-d`" flag.
curl -d 'username=admin&password=password' -L http://inlanefreight.com/login.php
cURL automatically sends a POST request when the "-d" flag is used. It's found that we're still on the login page even after specifying valid credentials. Let's use the "-v" switch to debug this
![[Pasted image 20210918090914.png]]
___
cURL -Cookie
---
The "--cookie" or "--cookie-jar" option can be used to specify cookie usage in cURL. This can point to /dev/null or a file on the disk, where the cookies will be saved.
curl --cookie cookies.txt http://inlanefreight.com/admin/dashboard.php -v
curl -d 'username=admin&password=password' -L --cookie-jar /dev/null  http://inlanefreight.com/login.php -v
______
Content-Type header
---
It's also possible to send JSON data using cURL. This can be done by specifying the "application/json" header with the "-H" flag.

curl -H 'Content-Type: application/json' -d '{ "username" : "admin", "password" : "password" }' --cookie-jar /dev/null -L  http://example/login.php
The "-H" option can be used to specify any header type.
_____
PUT and DELETE Methods
---
The "-X" option can be used to specify the request method to use.
![[Pasted image 20210918091346.png]]
___
cURL - File Upload and DELETE Method
---
![[Pasted image 20210918091509.png]]
We first create a file named test.txt. The "@" symbol is used by cURL to read the file and send it's contents as the data. As we can see, the file was uploaded successfully and retrieved later.

 curl -X DELETE http://inlanefreight.com/test.txt -vv
 ___
 Conclusion
 ---
In this module, we covered core concepts necessary for beginning to work with web applications such as understanding the differences between HTTP and HTTPS, an analysis of HTTP requests and responses, and an overview of key HTTP headers and methods and response codes. We took a deeper dive into the GET, POST, PUT, and DELETE methods with accompanying hands-on exercises. Finally, we covered the basics of the Burp Suite and cURL tools, which are two essential and powerful tools to have in our toolkits as we continue to work with web applications and begin enumerating and exploiting web application vulnerabilities in later modules. Throughout this module, we were provided with a wealth of external resources worth studying in-depth to gain a firm grasp over these core concepts that will come up again and again in our information security journey.