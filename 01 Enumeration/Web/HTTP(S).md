#   Enumerate version and few other details
 
-   If don't found the version, download the source code and grep the version. We might found the page that contains the version then.

-   Google their vulnerability
 
-  Default password login page

-  Guessing password login page?




# CMS
## WordPress
[[WordPress]]

### admin page 
/wp-admin

/wp-login

### config file 
setup-config.php

wp-config.php

### Enumerate users

/?author=1, /?author=2,

## Drupal


### Droopescan

```
droopescan scan drupal -u http://example.org/ -t 32
```

### Find version

/CHANGELOG.txt


## Adobe Cold Fusion

### Determine version

/CFIDE/adminapi/base.cfc?wsdl

## Elastix 

-   Google the vulnerabitlities
    

-   default login are `admin:admin` at `/vtigercrm/`
    

-   able to upload shell in profile-photo

## Joomla

 

-   Admin page - `/administrator`
    

```
   Configuration files
    
    configuration.php
    
    diagnostics.php
    
    joomla.inc.php
    
    config.inc.php
```
## Mambo



### Config files 

configuration.php

config.inc.php

# # Directory discovery
[[Directory discovery]]