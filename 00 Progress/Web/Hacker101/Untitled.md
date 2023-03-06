![[Pasted image 20230304152719.png]]
![[Pasted image 20230304153553.png]]
# Flag 1
creating new page
``
![[Pasted image 20230304152711.png]]
adding ' in username and password and we get a sql error
![[Pasted image 20230304152827.png]]
we can't simply use `' OR 1=1-- -` cos we get invalid password and in sql query in the error we saw that it stores password in somewhere else so we can try to do it this way
`' UNION SELECT "super" AS password#` and type super in password field
![[Pasted image 20230304154845.png]]
![[Pasted image 20230304154911.png]]
![[Pasted image 20230304154937.png]]

# Flag 2
editing page redirects me to /login
![[Pasted image 20230304160529.png]]
changing request method
![[Pasted image 20230304160603.png]]




# Flag 3

when trying to bypass login `' 1=1-- -'`
![[Pasted image 20230304160834.png]]
we can try to brute force password
![[Pasted image 20230304160933.png]]
using password list from `SecLists/Passwords/Common-Credentials/10-million-password-list-top-10000.txt`