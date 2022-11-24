	*check machine nics*
***
```
route print
apr -a 
ipconfig (win)
ifconfig(win)


ip route add <network_ip>/<cidr> via <gateway_ip>
```

*using metasploit*
```ruby
# On Metasploit
run autoroute -s SECOND_NIC_IP/24 # add IP to route table on metasploit
run autoroute -s SECOND_NIC_IP/24 # OR  -n 255.255.255.0

run autoroute -p # Check if command worked

background
# On Meterpreter

route add SECOND_NIC/24 SESSION_ID  # OR -n 255.255.255.0 Adds the route to my linux machine to run nmap or other scripts
# Running metasploit port scanner if we don't want to use nmap 

# We can use nmap on metasploit by typing nmap and run the command normaly if we used the last command
# Or we can use Metasploit port scan module 

use auxiliary/scanner/portscan/tcp 
set PORTS 80, 8080, 445, 21, 22 # Setting ports to scan
set RHOSTS 192.69.228.3-10 # Setting ip range for port scan
exploit


```



# Port Forward msf

 ```
session -i 1 

portfwd -h 

portfwd add -l LOCAL_PORT -p TARGET_PORT -r TARGET_IP

portfwd add -l 1234 -p 21 -r 192.106.221.3

```


# Chisel
[[Chisel]]
