# Live checking a file

```bash
tail -f FILE
```

# Check if someone is pinging me


```bash

sudo tcpdump -i tun0 icmp

```
# download directories recursive from web

```bash
wget --no-parent -r http://WEBSITE.com/
```

# watch 

```bash 
# Redo command every <TIME>
watch -n 1 'systemctl list-timers'
```





# Hex to text
```bash
xxd -r -p
```

# Check for file md5sum

```bash
md5sum FILE 
```


# Recover deleted files from mount files

```bash
strings MOUNT_FILE_DIRECTORY

xxd MOUNT_FILE | grep -v "000000 etc."
# also can grep 

grep -B2 -A2 -a '[a-z0-0]\{32\}' PATH # find flag 
https://www.youtube.com/watch?v=SRmvRGUuuno&ab_channel=IppSec

```




# Check mounts

```bash
df -lh
mount
```

# Check for Pwnkit

```
dpkg -s policykit-1
```


# Find type of encryption for /etc/shadow

```bash
grep -A 18 ENCRYPT_METHOD /etc/login.defs
```

# Open pictures from terminal 
```
eog
```
# Check listening ports

```
sudo lsof -i -P -n | grep LISTEN  
netstat -an | grep LISTEN  
ss -tulpn | grep LISTEN  
lsof -i:22 ## see a specific port such as 22 ##  
nmap -sTU -O IP-address-Here

```
# Services
```bash
service apache2 start # start apache services
service apache2 stop # stops service
systemctl enable # service will run when machine restarts 
```

## Viewing the Internet network services list
less /etc/services

# add host to /etc/hosts
```bash
echo "10.129.227.248 thetoppers.htb" | sudo tee -a /etc/hosts
```

# perm
  421
  rwx

# setuid search 
find  / -perm /4000 2>/dev/null ## to find all SUID files
find / -perm -u=s -type f 2>/dev/null



# keyboard shortcuts 
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
```
 '* sed -i -e 's/\r$//' FILE_NAME # *remove all
cat file1 file2... | { tr -d '\n'; echo; } > output.txt
 * tr -s '\n' ',' < test # *Replace all newlines
```

# Host discovery 
```bash
fping -gaq IP/RANGE


sudo netdiscover -r 192.168.1.0/24

nmap -sn 0.0.0.0/24

sudo arp-scan -I tap0 -g 0.0.0.0/24

zenmap  # GUI


# Local
arp -a # check for arp table
ip n # same as arp -a 


```

# ROT13 
echo 'fooman@example.com' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
# tar
tar xvf 
# openssl
openssl s_client -connect localhost:30001
# ncat port scan
nc -zvvw1 certifiedhacker.com 80-120

# directory with priv and clean tracks

```bash
/tmp
/dev/shm 
```
# Check linux version 
```bash
cat /proc/version
uname -r
hostnamectl | grep Kernel
lsb_release -a
```
# awk

```bash
# remove first line and print only 2nd line
awk -F\ '{print $#_OF_WAHT_I_WANT_}'


#remove 1 line from the end 
awk -F\] '{print $#_OF_WAHT_I_WANT_}'
```




# remove software
```bash
sudo apt --purge x
```

# run command as another user
```bash
sudo -u USER COMMAND
```



# Render websites command line

```bash
browsh --startup-url IP

lynx IP

```


# arpspoof

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward

arpspoof -i eth1 -t TARGET_IP -r TARGET_IP2

```

# WIFI

```bash
iwconfig # check wirless connection
```



# Metasploit 


```rb

-   post/linux/gather/enum_configs # check for config files
-   post/multi/gather/env # check env 
-   post/linux/gather/enum_network # network info - ssh etc. 
-   post/linux/gather/enum_protections # checks for system hardening techs
-   post/linux/gather/enum_system # enum system packages etc.
-   post/linux/gather/checkcontainer
-   post/linux/gather/checkvm
-   post/linux/gather/enum_users_history
-   post/multi/manage/system_session
-   post/linux/manage/download_exec
```