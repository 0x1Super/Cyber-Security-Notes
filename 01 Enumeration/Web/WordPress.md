# wpscan tool
```bash
wpscan --url [URL] -e u                                               to Enumerate wordpress 
 --plugins-version-detection aggressive -e ap                         enumurate plugins
wpscan --url [URL] -U admin -P wordlist                           bruteforce feature of WPScan
```
## enumurate users
?auth=1

```

## Random agent

```
wpscan --url http://cybear32c.lab/ --random-agent
```

# Zoom.py - enumerate WordPress users
python zoom.py -u <wordpress site>

```





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
