# LLMNR/NBT-NS *LLMNR poisoning*
The service utilize user's username and NTLMv2 hash when responded to

An attack *man in the middle* if the user wrote wrong domain to connect to if there is an attacker in the middle they can capture the traffic and cos LLMNR users user username and hash they send the creds along with it so attacker can capture the user's creds *using responder [[PNPT/AD/Attacking AD/Tools]]*

# Mitigation 
- To disable LLMNR, select "Turn OFF multicast Name Resolution" under local Computer Policy > Computer Configuration > Administrativce Templates > Network                                                   .> DNS Client in the Group Policy Editor

- To disable NBT-NSF, navigate to Network Connections 
  .> Network Adapter Properties > TCP/IPv4 Properties
  .>Advanced tab > WINS tab and select "Disable NetBIOS over TCP/IP".


If company must use or cannot disable LLMNR/NBT-NS< the best course of action is to:
- Require Network Access Control.
- Require strong user passwords (e.g., >14 characters in length and limit common word usage). The more complex and long the password< the harder it is for an attacker to crack the hash
