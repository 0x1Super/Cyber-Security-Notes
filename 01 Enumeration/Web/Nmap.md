[[04 Studies/Tools/Nmap]]
## AutoRecon

autorecon 10.10.10.3

autorecon -t targets.txt

## Initial scan TCP

nmap -sC -sV -O -oA nmap/initial 10.10.10.3

## Full scan TCP

nmap -sC -sV -O -p- -oA nmap/full 10.10.10.3

## Full scan UDP



nmap -sU -O -p- -oA nmap/udp 10.10.10.3

## Sparta

SPARTA is a python GUI application which simplifies network infrastructure penetration testing by aiding the penetration tester in the scanning and enumeration phase.

## Aggresive Scan

nmap -A $ip

## Scan for alive hosts

nmap -sn $ip/24

nmap -vvv -sn $ip/24

nmap -sn -n $ip/24 > ip-range.txt

## Scan specific IP range

$ nmap -sP 10.0.0.0-100

## Sort out the machines that are up


$ cat ip-range.txt | grep -B 1 "Host is up"

and now filter all the IPs and create a file.

```
grep -o '[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}' ip-range.txt > only-ip.txt
```



## Fast scanning

Scan 100 most common ports

nmap -F $ip

## Quick TCP Scan

nmap -sC -sV -vv -oA quick $ip

## Quick UDP Scan

nmap -sU -sV -vv -oA quick_udp $ip

## Full TCP Scan

nmap -sC -sV -p- -vv -oA full 10.10.10.10

## Port knock


for x in 7000 8000 9000; do nmap -Pn --host_timeout 201 --max-retries 0 -p $x $ip; done

## Scan deeply

Scanning more deeply:

nmap -v -p- -sT $ip

Example:

nmap -v -p- -sT 10.0.1.0/24


## Scan for specific port

nmap -p T:80,443,8080 $ip/24

- Use `-T`: specifies TCP ports. Use `-U`: for UDP ports.

## Scan for unused IP addresses and store in text file


nmap -v -sn $ip/24 | grep down | awk '{print $5}' > filename.txt

## Other option

nmap -sV -sC -v -oA output $ip

## UDP scan


- Scanning this might slow and unreliadble

nmap $ip -sU

Example:

$ nmap 10.11.1.X -sU

## Scan targets from a text file

Create a text file contains of our targets machine (like in method Scan for unused IP addresses and store in text file):

192.168.1.144

192.168.1.179

192.168.1.182

Run this nmap command with `-iL`

nmap -iL list-of-ips.txt

## Onetwopunch.sh


Grab the latest bash script

git clone https://github.com/superkojiman/onetwopunch.git

cd onetwopunch

Create a text file contains of our targets machine (like in method Scan for unused IP addresses and store in text file):

192.168.1.144

192.168.1.179

192.168.1.182

Then, run the script and tell it to read our txt file and perform TCP scan against each target.

./onetwopunch.sh -t ip-range.txt -p tcp

So, the idea behind the script to generate a scan of 65,535 ports on the targets. The script use unicornscan to scan all ports, and make a list of those ports that are open. The script then take the open ports and pass them to nmap for service detection.