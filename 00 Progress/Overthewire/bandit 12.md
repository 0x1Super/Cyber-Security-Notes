ssh bandit1@bandit.labs.overthewire.org -p 2220 PW: IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
Task: The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

ANS:
ls
file data.txt
mkdir /tmp/kekw
cd /tmp/kekw
cp ~/data.txt ./sok
file sok                                                                                                                                                    
  mv sok sok.gzip                                                                                                                                             
  unzip                                                                                                                                                       
  ls                                                                                                                                                          
  gunzip -d sok.gzip                                                                                                                                          
  unzip sok.gzip                                                                                                                                              
  unzip sok.gz                                                                                                                                                
  mv sok.gzip sok.gz                                                                                                                                          
  gzip -d sok.gz                                                                                                                                              
  ls                                                                                                                                                          
  file sok                                                                                                                                                    
  mv sok sok.bzip                                                                                                                                             
  mv sok sok.bz2                                                                                                                                              
  mv sok.bzip sok.zb2                                                                                                                                         
  bzip2 -d sok.zb2                                                                                                                                            
 bzip2 -d sokzb
  ls
  file sok.zb2.out
  mv sok.zb2.out sok.gz
  gzip -d sok.gz
  ls
  file sok
  ls
  mv sok sok.tar
  tar -xfr sok.tar
  ls
  tar -d sok.tar
  tar -fd sok.tar
  tar xvf sok.tar
file data5.bin
  mv data5.bin data5.tar
  tar xvf data5.tar
  file data
  file data6.bin
  mv data6.bin data.bz2
  bzip2 -d data.bz2
  ls
  file data
  mv data data.tar
  tar xvf data.tar
  file data8.bin
  mv data8.bin data8.gz
  gzip -d data8.gz
  ls
  file data8
  cat data8

#PW :  8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL