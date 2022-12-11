# How Kerberos works

1. Request TGT from DC  (provide NTLM hash)
2. Receive  TTFT enc w/ krbtgt hash
3. Request TGS for server (Presents TGT)
4. Receive TGS enc w/ server's account hash (TGS received )
5. Present TGS for service enc w/ server's account
6. Used when mutual authentication is required.

so if we have a user cred in the domain we can capture TGS with server's account
We can then try to crack the *sever hash that we got in step 4*

hashcat -m 13100
# Mitigation

- Strong Passwords
- Least privilege 