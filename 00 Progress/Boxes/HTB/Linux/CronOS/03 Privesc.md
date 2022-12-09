+fUFGL45d1YZYlSTc0Sm71wPzJejQN/K6s9bHHihdYE=
after looking around in /var/www/laravel 
we found hidden file .env contains [[00 Progress/Boxes/HTB/Linux/CronOS/04 Creds]]
let's run linpeas
![[Pasted image 20210930022001.png]]    
127.0.0.1:953
there is cron job that runs as root to run schedule artisan after some googling we find that we can create a schedule using https://laravel.com/docs/8.x/scheduling
first we need to find Kernel.php and edit it and edit into protected function section
```bash
find / -name Kernel.php 2>/dev/null
```
![[Pasted image 20221130002010.png]]


![[Pasted image 20221130002032.png]]



```
$schedule->exec('touch /ya_rap')->everyMinute();
```
using everyMinute();
in Kernel.php file 
![[Pasted image 20210930025510.png]]
![[Pasted image 20210930025641.png]]
#rootflag 1703b8a3c9a8dde879942c79d02fd3a0