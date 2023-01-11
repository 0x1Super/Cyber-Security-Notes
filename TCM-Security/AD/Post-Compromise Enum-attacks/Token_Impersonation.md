# Token impersonation
What are Tokens?
   - Temporary keys that allow you access to a system/network without having to provide credentials each time you access a file. (It's like cookies for computers)

 *Two Types:*
   - Delegate - Created for logging into a machine or using Remote Desktop
   - Impersonate - **non-interactive** such as attaching a network drive or a domain logon




# Mitigation

- Limit user/group token creation permissions
- Account tiering
- Local admin restriction



# Token impersonation with incognito


after getting a shell with psexec we can load *Incognito*

```
load incognito
help
list_tokens -u or -g # (to list the tokens on the machine)

impersonate_token THETOKEN

rev2self (to get back to the user u started with)
```


---
Tags: #delegation #Token_impersonation