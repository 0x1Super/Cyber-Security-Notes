Overview:
 - Group Policy Preferences allowed admins to create policies using embedded creds
 - These creds were encrypted and placed in a *cPassword*
 - The key was accidentally released
 - Patched in Ms14-025, but doesn't prevent previous uses
 - Groups.xml in SYSVOL

Invoke-GPP or msfconsole 