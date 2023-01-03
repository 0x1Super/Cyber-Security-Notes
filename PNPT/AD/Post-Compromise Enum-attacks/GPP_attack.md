Overview:
 - Group Policy Preferences allowed admins to create policies using embedded creds
 - These creds were encrypted and placed in a *cPassword*
 - The key was accidentally released
 - Patched in Ms14-025, but doesn't prevent previous uses
 - Groups.xml in SYSVOL
 - Works on windows server 2012

# Attack
- If a system is vulenrable to ms14-024 attacker can find Groups.xml inside SYSVOL share and find password under cpassword
 ![[Pasted image 20221227162341.png]]
- and then decrypt it 




![[PNPT/AD/Post-Compromise Enum-attacks/Tools#gpp / cPassword attack]]

---
Tags: #AD #gpp #MS14-025
Resources: