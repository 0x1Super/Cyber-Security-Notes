First we run our nmap scan 
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; prot
80/tcp open  http    nginx 1.14.0 (Ubuntu)
running gobuster got us nowhere and the website has nothing interesting running gobuster with vhost to check for subdomains we got api-prod.horizontall.htb 
running gobuster again on that subdomain we found 
/reviews              (Status: 200) [Size: 507]                         
/users                (Status: 403) [Size: 60]                          
/admin                (Status: 200) [Size: 854]                  
/Reviews              (Status: 200) [Size: 507]                    
/robots.txt           (Status: 200) [Size: 121] 
in robot.txt we found 
##To prevent search engines from seeing the site altogether, uncomment the next two lines:
##User-Agent: *
##Disallow: /
names : doe,wail,john

/admin has a strapi login page
after some research we found that under /admin/strapiversion will show us strapi version 
strapiVersion	"3.0.0-beta.17.4" 
which is a #vul version to 
Strapi 3.0.0-beta - Set Password (Unauthenticated)
https://www.exploit-db.com/exploits/50237
changing the url in the script and trying the email admin@horizontall.htb and BOM! we are in!
