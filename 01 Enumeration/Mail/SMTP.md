smtp-user-enum -M VRFY -U /root/Desktop/user.txt -t 192.168.1.107
smtp-user-enum -M VRFY -D mail.ignite.lab -u raj -t 192.168.1.107
ismtp -h 192.168.1.107:25 -e /root/Desktop/email.txt

# Metasploit

```ruby
-   auxiliary/scanner/smtp/smtp_version # smtp version
-   auxiliary/scanner/smtp/smtp_enum # enum users
-   auxiliary/scanner/smtp/
-   auxiliary/scanner/smtp/
```