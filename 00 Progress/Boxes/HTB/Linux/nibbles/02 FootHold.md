o checking the [[Shellshock]]again to see how to exploit the server
and upload pentest monkey php reverse shell through my image plugin
uploading the shell with the name image.php 
then we start a listener then we go to 
http://10.10.10.75/nibbleblog/content/private/plugins/my_image/image.php
and we are in ! 
python3 -c 'import pty;pty.spawn("/bin/bash")'
#userflag dc3784712e9259e9ec6335840ccb706e