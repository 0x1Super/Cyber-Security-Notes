- Web server was running on outdated opennetadmin which had a dangerous CVE which lead to command execution on the server and lead to getting a reverse shell 
 
- database password was same as a user ssh password                                                                                                                                                                                                                  
- server had a website on the server that had a user ssh private key which lead to getting into another user who has higher privileges on the server        
- joanna user had sudo privileges on a binary (nano) which lead to privileges escalation and getting to root access                                                                               