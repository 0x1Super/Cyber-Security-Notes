# WAF Web Application Firewall

WAF is a firewall works on layer 7 to protect the web applications from layer 7 attacks such as 
OWASP top 10 
HTTP flood
DDOS 
Slow loris 
and more 
### how it works
WAF designed to analyze each HTTP/S request at the application layer It is typically user, session, and application aware, cognizant of the web apps behind it and what services they offer 
it examines the request getting to web apps WAF start analyzing and checking if this request are legit and not  malicious 

## why we should use WAF 
If you have multiple different web apps servers and each are built differently it's hard to monitor configure and patch all of them separately  so we need a WAF that we can put in front of the web apps network and configure it to protect the servers 
and also using only FIREWALL won't protect the network from layer 7 attacks because fire walls works on layer 3-4 
