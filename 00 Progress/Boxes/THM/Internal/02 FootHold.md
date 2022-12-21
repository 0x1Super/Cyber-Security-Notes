first things first let's get a stable shell
python -c 'import pty;pty.spawn("/bin/bash")'
CTRL + Z
stty raw -echo
fg
export TERM=xterm

in /opt directory we found some [[00 Progress/Boxes/THM/Internal/04 Creds]]
we are in SSH 
now [[00 Progress/Boxes/THM/Internal/03 Privesc]]