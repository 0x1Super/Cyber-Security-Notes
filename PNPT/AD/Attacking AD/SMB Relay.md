# SMB Relay 
instead of cracking the target hashes we captured with responder we can instead relay the hashes to specific machines and potentially gain access
## Requirements 
- SMB signing disabled on the target or enabled but not required 
- Relayed user creds must be admin on the machine
- we can't send hashes from a machine to a same machine it must be admin on other machine


# Mitigation

- Enable SMB Signing on all devices
    - Pro: Completely stops the attack
    - Con: Can cause performance issues with file copies
- Disable NTLM authentication on network 
	- Pro: Completely stops the attack
    - Con: If Kerberos stops working, Windows defaults back to NTLM
- Account tiering:
	 - Pro: Limits domain admins to specific tasks (e.g. only log onto serves with need for DA)
    - Con: Enforcing the policy may be difficult 

- Local admin restriction
	- Pro: Can prevent a lot of lateral movement
	- Con: Potential increase in the amount of service desk tickets 


# Example


![[PNPT/AD/Attacking AD/Tools#SMB Relay]]