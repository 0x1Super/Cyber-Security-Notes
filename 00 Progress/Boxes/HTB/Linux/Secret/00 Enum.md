IP = 10.129.196.18
First we start our all ports nmap scan 
nmap $IP -p- -vvv

PORT     STATE SERVICE REASON
22/tcp   open  ssh     syn-ack
80/tcp   open  http    syn-ack
3000/tcp open  ppp     syn-ack
![[Pasted image 20211204082706.png]]
![[Pasted image 20211204083604.png]]
3000/tcp open  http    syn-ack Node.js (Express middleware)
22/tcp   open  ssh     syn-ack OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    syn-ack nginx 1.18.0 (Ubuntu)

checking out the website we see that we can download the source code 
while it's downloading let's see the page 
![[Pasted image 20211204090202.png]]
after downloading the source code it looks like a github repo so                                                             
we can try and 	use git extractor  to get more enumeration going 
checking out the results on .env file we get a token secret ![[Pasted image 20211207122528.png]]

now from what we see on the website documentations on how to register and login and verify 


let's try to use burp to send  POST request to the server and register 
![[Pasted image 20211204090808.png]]
looks like the user was created successfully 
trying to authenticate with 
![[Pasted image 20211207113937.png]]
and we get auth key 
eyeyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2MWFmMmI5ZmYyNGI0NTA0NzcxMTY4NzAiLCJuYW1lIjoic3VwZXJtZSIsImVtYWlsIjoic3VwZXJAcm9vdG0ubWUiLCJpYXQiOjE2Mzg4NzQzMjh9.ooMAsnkB20U5MWm3l9-I-Dr2n2noEeG3y_a1Dnn-QBY

now let's try to verify ourselves using auth-token: 
![[Pasted image 20211207114910.png]]
looks like we are a normal user 
![[Pasted image 20211207114940.png]]
after some digging in the source code we found that 
![[Pasted image 20211207130309.png]]
looks like it checks for user if it's theadmin that gives us admin priv so what we can do is forget a 
we can use the secret earlier to create a forged token with the name theadmin 
'gXr67TtoQL8TShUc8XYsK2HvsBYfyQSFCFZe4MQp7gRpFuMkKjcM72CNQN4fMfbZEKx4i7YiWuNAkmuTcdEriCMm9vPAYkhpwPTiuVwVhvwE' <- secret
let's use https://jwt.io/ to make our token 
first let's get our original token ![[Pasted image 20211207122813.png]]
now let's add the new name and the secret
now let's try to curl to test our forged token 
![[Pasted image 20211207122914.png]]
looks like it works!
now let's dig depper by running gobuster on /api directory
we found that there's a /logs 
![[Pasted image 20211207130014.png]]
we found that we get Access Denied from the server which probably means we need to authinticate using the jwt token 
![[Pasted image 20211207130104.png]]
