default port 873

# What is Rsync
The rsync protocol is generally preferred. The changes that need to get transfered are called deltas. Using deltas to update files is an extremely efficient way to reduce the required bandwidth for the transfer as well as the required time for the transfer to complete.


It often happens that rsync is misconfigured to permit anonymous login, which can be exploited by an attacker to get access to sensitive information stored on the remote machine.


# Commands
```bash
rsync [OPTION] … [USER@]HOST::SRC [DEST]

```


 ## List files
```bash
rsync --list-only {target_IP}:: # List the files inside  
rsync --list-only {target_IP}::DIRECTORY


rsync rsync://{target_IP}/
rsync rsync://{target_IP}/DIRECTORY

```

## copy files 

```
rsync rsync://{target_IP}/FILE OUTPUT_FILE

rsync {target_IP}::/FILE OUTPUT_FILE
```