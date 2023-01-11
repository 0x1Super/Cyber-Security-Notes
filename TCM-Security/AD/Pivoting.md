# check machine nics
***
```
route print
arp -a 
ipconfig (win)
ifconfig(win)


ip route add <network_ip>/<cidr> via <gateway_ip>
```

# using metasploit
```ruby
run arp_scanner IP/24

# On Meterpreter
run autoroute -s SECOND_NIC_IP/24 # add IP to route table on metasploit
run autoroute -s SECOND_NIC_IP/24 # OR  -n 255.255.255.0

run autoroute -p # Check if command worked

route print # check route 

background
# On Metasploit 

route add SECOND_NIC/24 SESSION_ID  # OR -n 255.255.255.0 Adds the route to my linux machine to run nmap or other scripts
# Running metasploit port scanner if we don't want to use nmap 


resolve # resolves domains to ips 
```



## Port Forward msf

 ```ruby
session -i 1 

portfwd -h 

portfwd add -l LOCAL_PORT -p TARGET_PORT -r TARGET_IP

portfwd add -l 1234 -p 21 -r 192.106.221.3

# USE bind shells instead of reverse when connecting to 2nd target
```


## Proxy
```ruby
use auxiliary/server/socks_proxy

set VERSION 4a 

set srvhost 127.0.0.1

set SRVPORT 9050

run -j
# Run all commands with proxychains after this
# use -sT with nmap 

```


# Chisel
[[03 Commands/Linux/Pivoting]]
