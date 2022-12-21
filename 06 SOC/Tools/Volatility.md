
# what is 
[Volatility](What_is_Volatility) 



## usage
```bash

python3 vol.py /path/to/my/memorydump.vmem plugin

```




## flags

```bash
-f # name and location of the memory dump that you wish to analyse.

-v # increases the verbosity of Volatility

-p #  allows you to override the default location of where plugins are stored.

-o # allows you to specify where extracted processes or DLLs are stored.

```

## plugins

 [Rest_of_the_plugins](https://volatility3.readthedocs.io/en/stable/volatility3.plugins.windows.html)
 

| Plugin | Description | Objective |
|---------|--------------|------------|
|windows.pslist|This plugin lists all of the processes that were running at the time of the capture.|To discover what processes were running on the system.|
|windows.psscan|This plugin allows us to analyse a specific process further.|To discover what a specific process was actually doing.|
|windows.dumpfiles|This plugin allows us to export the process, where we can perform further analysis (i.e. static or dynamic analysis).|To export a specific binary that allows us further to analyse it through static or dynamic analysis.|
|windows.netstat|This plugin lists all network connections at the time of the capture.|To understand what connections were being made. For example, was a process causing the computer to connect to a malicious server? We can use this IP address to implement defensive measures on other devices. For example, if we know an IP address is malicious, and another device is communicating with it, then we know that device is also infected.|





### examples

```bash


# First, let's use the `imageinfo` plugin to analyse our memory dump file to determine the Operating System
python3 vol.py -f workstation.vmem windows.info

python3 vol.py -f workstation.vmem windows.pslist # list running processes 

python3 vol.py -f workstation.vmem windows.psscan # iscover what a specific process was actually doing.

python3 vol.py -f workstation.vmem windows.dumpfiles --pid 4640 # understand what connections were being made. for processes ID 4640


```