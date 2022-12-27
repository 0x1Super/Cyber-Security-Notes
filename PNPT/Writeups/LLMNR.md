


# LLMNR/NBT-NS *LLMNR poisoning*

## What is LLMNR:

LLMNR is known as Link Local Multicast and Name Resolution it's basically DNS, it's used to identify hosts when DNS fails 

The service utilize user's username and NTLMv2 hash when 

LLMNR poisoning is **man in the middle** attack if the user tried accessing wrong domain or share ( anything causing DNS issues ) and the server sends broadcast message to look for that domain if there is an attacker in the middle they can capture the traffic and and tell the server that we know where that domain is and because LLMNR uses user's username and hash attacker can capture the user's creds [using responder](#^b5ca2f)

## Mitigation 
- To disable LLMNR, select "Turn OFF multicast Name Resolution" under local Computer Policy > Computer Configuration > Administrativce Templates > Network                                                   .> DNS Client in the Group Policy Editor

- To disable NBT-NSF, navigate to Network Connections 
  .> Network Adapter Properties > TCP/IPv4 Properties
  .>Advanced tab > WINS tab and select "Disable NetBIOS over TCP/IP".


*If company must use or cannot disable LLMNR/NBT-NS< the best course of action is to:*
- Require Network Access Control.
- Require strong user passwords (e.g., >14 characters in length and limit common word usage). The more complex and long the password< the harder it is for an attacker to crack the hash


# Responder

Responder is a part of [impacket](https://github.com/fortra/impacket) toolkit and we will use it to respond to LLMNR requests 

## Usage


```bash
python Responder.py -I eth0 -dwv

-I # interface 
-w # Start the WPAD rogue proxy server
-d # Enable answers for DHCP broadcast requests.
-v # verbose, shows hash twice 
```

^b5ca2f
<div style="page-break-after: always;"></div>

##  SMB attack

when a user tries to access a share that doesn’t exist,
the system sends out an LLMNR query to the entire network.
Asking if any user(IP address) has access to that share 
Attacker can poison LLMNR request using responder and tells the victim to send it the hash and we will authenticate you to xdd share, and then responder capture the user's credentials 

![[Pasted image 20221223163516.png]]

![[Pasted image 20221223164354.png]]
trying access a share that doesn't exist on the target 


```
sudo responder -I eth0 -v 
```
Now after running responder we wait for the victim to try to access a share that doesn't exist 
![[Pasted image 20221223163625.png]]
![[Pasted image 20221223163653.png]]
as u can see above we got username IPv6, username and password hash
## WPAD

### What is WPAD

[**WPAD:**](https://www.pcworld.com/article/415991/disable-wpad-now-or-have-your-accounts-and-private-data-compromised.html) Web Proxy Autodiscovery Protocol is a method used by a browser to automatically locate and interface with cache services in a network so that information is delivered quickly. WPAD by default uses DHCP to locate a cache service to facilitate straightforward connectivity and name resolution.

### How it works

When a user enters an invalid URL is an input in the browser,
the browser fails to load that page using DNS and hence,
sends out an LLMNR request to find a WPAD proxy server.
Responder (LLMNR poisoner) creates a rogue WPAD proxy server, poisons the request and tells the browser that it knows where that url is and when the browser tries to authenticate to responder, responder captures the user's hash

### Attack


```bash
python Responder.py -I eth0 -dwv
```

![[Pasted image 20221223170242.png]]
now that we have our responder on WPAD proxy and DHCP poisoning are on
when a user enters wrong URL, responder  poisons and injects DHCP response with WPAD’s IP

![[Pasted image 20221223153436.png]]

As soon as the client inputs his credentials, we receive their NTLM hashes!

![[Pasted image 20221223153544.png]]

# Hashcat 

After capturing the NTLM hash what an attacker can do is crack that hash using tools like **Hashcat** 

First we save the hash we got to a file 
Then we run hashcat using rockyou.txt passwords 

```bash
hashcat -m 5600 hash /usr/share/wordlists/rockyou.txt
-m # hash type 
```

![[Pasted image 20221223154958.png]]
![[Pasted image 20221223154924.png]]

 <br> <br> <br> <br> <br> <br> <br> <br>
  <br> <br> <br> <br>  <br> <br>
  ---
  Tags: #AD #SMB #LLMNR #MITM #responder 