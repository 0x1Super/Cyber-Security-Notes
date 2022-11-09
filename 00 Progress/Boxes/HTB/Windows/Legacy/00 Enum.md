# nmap scan
139/tcp  open   netbios-ssn   syn-ack      Microsoft Windows netbios-ssn                                                                                      
445/tcp  open   microsoft-ds  syn-ack      Windows XP microsoft-ds                                                                                            
3389/tcp closed ms-wbt-server conn-refused    

 smb-os-discovery:                                                                                                                                           
|   OS: Windows XP (Windows 2000 LAN Manager)                                                                                                                 


# nmap script scan 
VULNERABLE:                                                                                                                                               
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)                                                                                 
|     State: VULNERABLE                                                                                                                                       
|     IDs:  CVE:CVE-2017-0143                                                   