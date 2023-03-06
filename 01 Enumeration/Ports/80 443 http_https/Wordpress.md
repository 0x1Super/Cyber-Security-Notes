# wpscan tool
```bash
wpscan --url [URL] -e u # to Enumerate wordpress 
 --plugins-version-detection aggressive -e ap  # enumurate plugins
wpscan --url [URL] -U admin -P wordlist # bruteforce 

--enumerate p,t,u 


wp-content-dir wp-content --plugins-detection aggressive # plugins enum if first didn't work

# Flags
-p # plugins
-u # users
--disable-tls-checks # for TLS
--password-attack wp-login # bruteforce type wp-login

```


## API

```bash
# I got for free at the wpscan site. I store my API token in the Bash environment variable $WPSCAN_API with this line in my ~/.bashrc file:

echo export WPSCAN_API="Blqhlhxam73xEphoePsGvjsIZ0RjwhWUuP0TUoDtsyw" >> ~/.bashrc


wpscan --url http://office.paper --api-token $WPSCAN_API


```



## enumurate users



```
?auth=1
## Random agent
wpscan --url http://cybear32c.lab/ --random-agent


# Zoom.py - enumerate WordPress users
python zoom.py -u <wordpress site>

```

## config file

setup-config.php

wp-config.php



# Get Plugins

```
curl -s -X GET https://wordpress.org/support/article/pages/ | grep -E 'wp-content/plugins/' | sed -E 's,href=|src=,THIIIIS,g' | awk -F "THIIIIS" '{print $2}' | cut -d "'" -f2
```


# Get Themes

```
curl -s -X GET https://wordpress.org/support/article/pages/ | grep -E 'wp-content/themes' | sed -E 's,href=|src=,THIIIIS,g' | awk -F "THIIIIS" '{print $2}' | cut -d "'" -f2
```

# Extract versions in general



```
curl -s -X GET https://wordpress.org/support/article/pages/ | grep http | grep -E '?ver=' | sed -E 's,href=|src=,THIIIIS,g' | awk -F "THIIIIS" '{print $2}' | cut -d "'" -f2
```
