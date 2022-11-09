#userflag e6710a5464769fd5fcd216e076961750

Linux version 3.2.0-23-generic 
Sudo version 1.8.3p1                                                                                                                                          
127.0.0.1:631
PermitRootLogin yes                                                                                                                                           
root       1180  0.0  0.1  26416  1672 ?        Ss   05:17   0:00 /usr/bin/tmux -S /.devs/dev_sess                                                            
/usr/bin/X
looking through home directory we found that .bash_history has some content
![[Pasted image 20211216181717.png]]
looks like the user hype with running tmux copy the command we found
tmux -S /.devs/dev_sess 
and we are root
![[Pasted image 20211216181800.png]]

#rootflag f1bb6d759df1f272914ebbc9ed7765b2