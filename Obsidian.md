

# Page break

```xml
<div style="page-break-after: always;"></div>
```


# Reference blocks 
```
^SMTH 

^SMTH

^^SMTH
[[#^ss]]
```

# New Line

```
 <br>
```

# Image size

```
[[Pasted image 20221124151156.png | SIZE_IN_PIXELS OR RATIO 500x400]]
```

# pdf page

```
![[smth.pdf#page=1]]
```

# tables

```
| | |
|-|-|
```



```python
import os
import sys
import socket
import subprocess

def validate_ip(ip):
    try:
        socket.inet_aton(ip)
    except socket.error:
        print("Invalid IP address")
        return False
    return True

def validate_port(port_str):
    try:
        port = int(port_str.strip())
        if port < 1 or port > 65535:
            raise ValueError
    except:
        print("Invalid port: " + port_str.strip())
        return False
    return True

def get_open_ports(ip, ports):
    open_ports = []
    for port in ports:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.settimeout(1)
        try:
            s.connect((ip, port))
            open_ports.append(port)
            s.close()
        except:
            pass
    return open_ports

# Get user input for IP address and ports
ip = input("Enter IP address to scan (e.g. 192.168.1.1): ")
ports_str = input("Enter ports to scan (e.g. 80, 443): ")

# Validate the IP address
if not validate_ip(ip):
    sys.exit()

# Validate the ports
ports = []
ports_str_list = ports_str.split(",")
for port_str in ports_str_list:
    if not validate_port(port_str):
        sys.exit()
    ports.append(int(port_str.strip()))

# Check if any valid ports were entered
if not ports:
    print("No valid ports entered")
    sys.exit()

# Check if the ports are open for the given IP address
alive_ports = get_open_ports(ip, ports)

# Print the list of open ports for the given IP address
if alive_ports:
    print("Open ports for " + ip + ": " + str(alive_ports))
else:
    print("No open ports found for " + ip + " on specified ports")


```