
# Privesc
find . -perm /4000 ## to find all SUID files
find / -perm -u=s -type f 2>/dev/null



# copy to clipboard 
cat or echo FILE | xclip -selection c


CTRL + K kill to the end of the line
CTRL + U kill to the beginning of the line 
CTRL + Y returns what got killed
CTRL + W kill 1 back word word 
CTRL +xe go to editor 
ALT + . paste last argument 
 fc open last command in editor
 disown -a && exit close terminal keep child processes 
  CTRL + R reverse search command CTRL + R  again search again 
   CTRL  + G in CTRL + R *reverse search* get out 
 CTRL + r to search in history 

# replace words 
*remove all '* sed -i -e 's/\r$//' FILE_NAME
cat file1 file2... | { tr -d '\n'; echo; } > output.txt
*Replace all newlines * tr -s '\n' ',' < test

# ping all alive users
fping -gaq IP/RANGE
sudo netdiscover -r 192.168.1.0/24

# ROT13 
echo 'fooman@example.com' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
# tar
tar xvf 
# openssl
openssl s_client -connect localhost:30001
# ncat port scan
nc -zvvw1 certifiedhacker.com 80-120

# directory with priv and clean tracks
/dev/shm 

# Check linux version 
cat /proc/version
uname -r
hostnamectl | grep Kernel
lsb_release -a

# awk
remove first line and print only 2nd line
awk -F\ '{print $#_OF_WAHT_I_WANT_}'
remove 1 line from the end 
awk -F\] '{print $#_OF_WAHT_I_WANT_}'

# check for open ports 
ss -lntp

# remove softwares
sudo apt-get --purge x