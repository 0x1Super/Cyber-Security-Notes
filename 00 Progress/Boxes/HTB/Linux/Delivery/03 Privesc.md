now what we can do is look for the database and see if we can get creds to the database and dump some username and passwords 
![[Pasted image 20211208173238.png]]
![[Pasted image 20211208173344.png]]
looks like we got a hit 
mmuser:Crack_The_MM_Admin_PW
![[Pasted image 20211208173629.png]]
and we are in database
![[Pasted image 20211208173709.png]]
now let's get the tables and see what's inside the table Users

![[Pasted image 20211208173733.png]]

let's now dump everything inside Username and Password 
![[Pasted image 20211208174119.png]]
now we can try to crack the password using hashcat and rules and from earlier we knew that the company uses PleaseSubscribe! as a password variant 
![[Pasted image 20211208163559.png]]
so let's try to crack the password using hashcat rules first let's hashid to see what type  of hash is it ![[Pasted image 20211208175110.png]]  
now let's run hashcat --example-hashes
![[Pasted image 20211208175233.png]]
now let's crack the root password
hashcat -m 3200 hash pw -r /usr/share/hashcat/rules/best64.rule 
![[Pasted image 20211208175356.png]]
and we have the password
root:PleaseSubscribe!21
![[Pasted image 20211208175458.png]]
#rootflag 5f2507bb3f8eac81cf54beab905100d2
