
# Server1

```
Nmap scan report for server1.ine.local (192.208.95.3)                                                                        
Host is up (0.0000090s latency).                                                                                             
Not shown: 999 closed tcp ports (reset)                                                                                      
PORT   STATE SERVICE VERSION                                                                                                 
80/tcp open  http    Werkzeug httpd 0.9.6 (Python 2.7.13)                                                                    
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).                                                         
|_http-favicon: Unknown favicon MD5: 8AE97B9D0E936309B9C237DA679511B7                                                        
| http-methods:                                                                                                              
|_  Supported Methods: GET HEAD POST OPTIONS                                                                                 
|_http-server-header: Werkzeug/0.9.6 Python/2.7.13                                                                           
MAC Address: 02:42:C0:D0:5F:03 (Unknown) 
```


## Open ports
80/tcp








# Server 2

```
Nmap scan report for server2.ine.local (192.208.95.4)                                                                        
Host is up (0.000010s latency).                                                                                              
Not shown: 999 closed tcp ports (reset)                                                                                      
PORT     STATE SERVICE VERSION                                                                                               
3306/tcp open  mysql   MySQL 5.5.62-0ubuntu0.14.04.1                                                                         
| mysql-info:                                                                                                                
|   Protocol: 10
|   Version: 5.5.62-0ubuntu0.14.04.1
|   Thread ID: 45
|   Capabilities flags: 63487
|   Some Capabilities: LongColumnFlag, ConnectWithDatabase, Support41Auth, LongPassword, Speaks41ProtocolNew, SupportsLoadDat
aLocal, Speaks41ProtocolOld, SupportsCompression, IgnoreSpaceBeforeParenthesis, IgnoreSigpipes, SupportsTransactions, Interac
tiveClient, ODBCClient, DontAllowDatabaseTableColumn, FoundRows, SupportsAuthPlugins, SupportsMultipleStatments, SupportsMult
ipleResults
|   Status: Autocommit
|   Salt: 4\ToR-'zV|.nb"{i}X#@
|_  Auth Plugin Name: mysql_native_password
MAC Address: 02:42:C0:D0:5F:04 (Unknown)
```



## Open ports
3306/tcp mysql 5.5.62


# Server 3 
```
Nmap scan report for server3.ine.local (192.208.95.5)
Host is up (0.0000090s latency).
Not shown: 998 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 e1:c9:8e:a0:ca:07:1d:e9:65:06:f2:8e:cd:51:fa:76 (RSA) 
|   256 82:26:cc:66:66:5b:29:7a:82:85:95:c2:43:a0:d4:6a (ECDSA)
|_  256 a9:85:9f:da:86:52:af:8d:ca:43:39:89:fa:9c:59:11 (ED25519)
8080/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
| http-methods: 
|   Supported Methods: GET HEAD POST PUT DELETE OPTIONS
|_  Potentially risky methods: PUT DELETE
|_http-title: Site doesn't have a title (text/html).
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: Apache-Coyote/1.1
MAC Address: 02:42:C0:D0:5F:05 (Unknown)
```


## Open ports

22/tcp

8080/tcp Tomcat 