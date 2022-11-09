now we can try to brute force ssh with username list and password list
[[Hydra]]
we found some [[00 Progress/Boxes/HTB/Linux/Beep/04 Creds]] after showing the page source code
![[Pasted image 20210924163716.png]]
and we are in elastix!
[[00 Progress/Boxes/THM/Internal/02 FootHold]]
after doing hydra with password list we gathered from the lFI we found that ssh also has same password as elastix
first after trying to ssh i got a diffie hellman error
Unable to negotiate with 10.129.228.29 port 22: no matching key exchange method found. Their offer: diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1

we solved that by using 
└─$ ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 root@10.129.228.29
alright we are in 
well we are in as root? xd
#rootflag 29a33eb8c3f2c3de223943ef693f8cd5
#userflag b246c30588841313c74b10b029813526