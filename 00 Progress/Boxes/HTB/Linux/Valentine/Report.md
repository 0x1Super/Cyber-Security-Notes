run port scan
IP = 10.129.247.220
# port scan
22/tcp  open  ssh      syn-ack OpenSSH 5.9p1 Debian 5ubuntu1.10 (Ubuntu Linux; protocol 2.0)                                                                  
80/tcp  open  http     syn-ack Apache httpd 2.2.22 ((Ubuntu))
443/tcp open  ssl/http syn-ack Apache httpd 2.2.22


running gobuster and nikto 
gobuster dir -u valentine.htb -w /opt/SecLists/Discovery/Web-Content/raft-medium-directories.txt -x php,txt

### port 80 enum
![[Pasted image 20211216183951.png]]
/dev
found notes.txt and a hype key
![[Pasted image 20211215200833.png]]
![[Pasted image 20211215200855.png]]
going into cyber chef to decode the key and it looks like a Hex 
![[Pasted image 20211215201044.png]]



-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,AEB88C140F69BF2074788DE24AE48D46

DbPrO78kegNuk1DAqlAN5jbjXv0PPsog3jdbMFS8iE9p3UOL0lF0xf7PzmrkDa8R
5y/b46+9nEpCMfTPhNuJRcW2U2gJcOFH+9RJDBC5UJMUS1/gjB/7/My00Mwx+aI6
0EI0SbOYUAV1W4EV7m96QsZjrwJvnjVafm6VsKaTPBHpugcASvMqz76W6abRZeXi
Ebw66hjFmAu4AzqcM/kigNRFPYuNiXrXs1w/deLCqCJ+Ea1T8zlas6fcmhM8A+8P
OXBKNe6l17hKaT6wFnp5eXOaUIHvHnvO6ScHVWRrZ70fcpcpimL1w13Tgdd2AiGd
pHLJpYUII5PuO6x+LS8n1r/GWMqSOEimNRD1j/59/4u3ROrTCKeo9DsTRqs2k1SH
QdWwFwaXbYyT1uxAMSl5Hq9OD5HJ8G0R6JI5RvCNUQjwx0FITjjMjnLIpxjvfq+E
p0gD0UcylKm6rCZqacwnSddHW8W3LxJmCxdxW5lt5dPjAkBYRUnl91ESCiD4Z+uC
Ol6jLFD2kaOLfuyee0fYCb7GTqOe7EmMB3fGIwSdW8OC8NWTkwpjc0ELblUa6ulO
t9grSosRTCsZd14OPts4bLspKxMMOsgnKloXvnlPOSwSpWy9Wp6y8XX8+F40rxl5
XqhDUBhyk1C3YPOiDuPOnMXaIpe1dgb0NdD1M9ZQSNULw1DHCGPP4JSSxX7BWdDK
aAnWJvFglA4oFBBVA8uAPMfV2XFQnjwUT5bPLC65tFstoRtTZ1uSruai27kxTnLQ
+wQ87lMadds1GQNeGsKSf8R/rsRKeeKcilDePCjeaLqtqxnhNoFtg0Mxt6r2gb1E
AloQ6jg5Tbj5J7quYXZPylBljNp9GVpinPc3KpHttvgbptfiWEEsZYn5yZPhUr9Q
r08pkOxArXE2dj7eX+bq65635OJ6TqHbAlTQ1Rs9PulrS7K4SLX7nY89/RZ5oSQe
2VWRyTZ1FfngJSsv9+Mfvz341lbzOIWmk7WfEcWcHc16n9V0IbSNALnjThvEcPky
e1BsfSbsf9FguUZkgHAnnfRKkGVG1OVyuwc/LVjmbhZzKwLhaZRNd8HEM86fNojP
09nVjTaYtWUXk0Si1W02wbu1NzL+1Tg9IpNyISFCFYjSqiyG+WU7IwK3YU5kp3CC
dYScz63Q2pQafxfSbuv4CMnNpdirVKEo5nRRfK/iaL3X1R3DxV8eSYFKFL6pqpuX
cY5YZJGAp+JxsnIQ9CFyxIt92frXznsjhlYa8svbVNNfk/9fyX6op24rL2DyESpY
pnsukBCFBkZHWNNyeN7b5GhTVCodHhzHVFehTuBrp+VuPqaqDvMCVe1DZCb4MjAj
Mslf+9xK+TXEL3icmIOBRdPyw6e/JlQlVRlmShFpI8eb/8VsTyJSe+b853zuV2qL
suLaBMxYKm3+zEDIDveKPNaaWZgEcqxylCC/wUyUXlMJ50Nw6JNVMM8LeCii3OEW
l0ln9L1b/NXpHjGa8WHHTjoIilB5qNUyywSeTBF2awRlXH9BrkZG4Fc4gdmW/IzT
RUgZkbMQZNIIfzj1QuilRVBm/F76Y/YMrmnM9k/1xSGIskwCUQ+95CGHJE8MkhD3
-----END RSA PRIVATE KEY-----

ound
/encode 
/decode

# Foothold 
we tried to do a stored xss attack with 
src=http://10.10.16.9
![[Pasted image 20211215204959.png]]
and we get a hit!
we couldn't do much with what we found but we ran 
nmap --script vuln and we got something interesting
a  exploit on ssl called heart bleed
![[Pasted image 20211215223036.png]]
running heartbleed we leaked some interesting info 
looks like someone is typing a base64 string into /decode 
https://www.exploit-db.com/exploits/32745 #heartbleed 
![[Pasted image 20211215225404.png]] it looks like a password
heartbleedbelievethehype
now let's try to ssh with the key we found earlier and the password we found now and i assume the username will be hype because the key was called hype.key
![[Pasted image 20211215230803.png]]
we are in !


# Privilege escalation 
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

| Service | Username | password                 |
| ------- | -------- | ------------------------ |
| ssh     | hype     | heartbleedbelievethehype |

    # Conclusion 
>- the server had an open directory that had a id_rsa ssh key to a user in the server
>- the server was vulnerable heartbleed exploit which lead to reading from memory the password for the ssh key user hype that we found earlier
>- the kernel version of the target was so out dated that it was vulnerable to most common exploits like dirty cow which lead to priv escalation 
>- the root had an open tmux session that the user hyper could connect to which lead to priv escalation 