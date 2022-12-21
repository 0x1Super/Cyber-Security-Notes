there is a george directory which has user flag 
#userflag edecbc9d5ad60861381afc5f7be24e98
looking around in /var/www/torrent/config.php file we found database [[00 Progress/Boxes/HTB/Linux/popcorn/04 Creds]]
$CFG->dbName = "torrenthoster";       //db name                                                                                                             
  $CFG->dbUserName = "torrent";    //db username                                                                                                              
  $CFG->dbPassword = "SuperSecret!!";   //db password 
  
  we are in database torrenthoster and we found a Admin password![[Pasted image 20210926160232.png]]
  it's MD5 encryption let's try to crack it 
  
  
  
