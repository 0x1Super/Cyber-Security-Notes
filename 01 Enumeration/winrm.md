
[Windows Remote Management](https://msdn.microsoft.com/en-us/library/windows/desktop/aa384426(v=vs.85).aspx) (WinRM) is a Microsoft protocol that **allows remote management of Windows machines** over HTTP(S) using SOAP. On the backend it's utilising WMI, so you can think of it as an HTTP based API for WMI.

If WinRM is enabled on the machine, it's trivial to remotely administer the machine from PowerShell. In fact, you can just drop in to a remote PowerShell session on the machine (as if you were using SSH!)

The easiest way to detect whether WinRM is available is by seeing if the port is opened. WinRM will listen on one of two ports:

-   **5985/tcp (HTTP)**
    

-   **5986/tcp (HTTPS)**
    

If one of these ports is open, WinRM is configured and you can try entering a remote session.

# Evilwirm
```
evil-winrm.rb -u USER -p PASS -i IP
```

# logging in using priv/pub keys

evil-winrm -S (SSL) -k (priv key file) -c (pub key file ) -i IP


# crackmapexec.py

```bash
crackmapexec <protocol> <target(s)> -u username -p password

crackmapexec.py winrm <ip address>  # to determine if the target has PSRemoting enabled.

crackmapexec.py winrm <ip address> -u USER -p PASS -x "sysinfo" # RCE

# Brute force

rackmapexec <protocol> <target(s)> -u ~/file_containing_usernames -p ~/file_containing_passwords

crackmapexec <protocol> <target(s)> -u ~/file_containing_usernames -H ~/file_containing_ntlm_hashes



```

# Metasploit

```ruby
use auxiliary/scanner/winrm/winrm_login # bruteforce

use exploit/windows/winrm/winrm_script_exec # login 

set force_vbs true


```