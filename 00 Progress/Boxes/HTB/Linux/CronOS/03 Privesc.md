+fUFGL45d1YZYlSTc0Sm71wPzJejQN/K6s9bHHihdYE=
after looking around in /var/www/laravel 
we found hidden file .env contains [[00 Progress/Boxes/HTB/Linux/CronOS/04 Creds]]
let's run linpeas
![[Pasted image 20210930022001.png]]    
127.0.0.1:953
there is cron job that runs as root to run schedule artisan after some googling we find that we can create a schedule using https://laravel.com/docs/8.x/scheduling
$schedule->exec('node /home/forge/script.js')->daily();
in Kernel.php file 
![[Pasted image 20210930025510.png]]
![[Pasted image 20210930025641.png]]
#rootflag 1703b8a3c9a8dde879942c79d02fd3a0