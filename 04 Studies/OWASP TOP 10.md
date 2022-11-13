
# `1-  Broken access Control`
Access control  is the application of constraints on who (or what) can perform attempted actions or access resources that they have requested. In the context of web applications

## **Cause**

- Broken access control vulnerabilities exist when a user can in fact access some resource or perform some action that they are not supposed to be able to access.

## **Prevention**:

- Access control mechanism  
- Record ownership 
- Log access control failure
- Rate limit APIs
- Least Privileges 


## **Examples**:

- Violation of the principle of least privilege or deny by default
- Bypassing access control checks by modifying the URL
- Permitting viewing or editing someone else's account
- Accessing API with missing access controls for POST, PUT and DELETE.
- Elevation of privilege



# `2- Cryptographic Failures`
## **Description**

The first thing is to determine the protection needs of data in transit and at rest. For example, passwords, credit card numbers, health records, personal information, and business secrets require extra protection

## **Cause:**
- encryption not enforced
- Using weak  cryptographic algorithms or protocols used either by default or in older code
- Data transmitted in clear text
- server certificate and the trust chain  not validated
- Are passwords being used as cryptographic keys in absence of a password base key derivation function?
- Are deprecated hash functions such as MD5 or SHA1 in use, or are non-cryptographic hash functions used when cryptographic hash functions are needed?
- Weak Cipher suite 

## **Prevention**

- Classify data processed, stored, or transmitted, Identify which data is sensitive and needs more protection
- Discard unnecessary  sensitive data as soon as possible 
- Encrypt all sensitive data 
- Ensure up to date algorithms, protocols and keys 
- Proper key management 
- Encrypt all data in transit with secure protocols such as TLS with forward secrecy (FS) ciphers, cipher prioritization by the server, and secure parameters. Enforce encryption using directives like HTTP Strict Transport Security (HSTS).
- Disable caching for  sensitive data.
- Do not use legacy or unsafe, not up to date protocols such as FTP and SMTP
- Always use authenticated encryption instead of just encryption.

## **Examples**:

- Downgrade attacks
- Cookie Theft 
- Sniffing/ MITM attacks
- Injection attacks



# `3- Injection`
## **Description**


Almost any source of data can be an injection vector, environment variables, parameters, external and internal web services, and all types of users. Injection flaws occur when an attacker can send hostile data to an interpreter.

## **Cause**

- User-supplied data is not validated, filtered, or sanitized by the application.
- Dynamic queries or non-parameterized calls without context-aware escaping are used directly in the interpreter
- Hostile data is used within object-relational mapping (ORM) search parameters to extract additional, sensitive records

## **Prevention**

- The preferred option is to use a safe API, which avoids using the interpreter entirely, provides a parameterized interface
- Use positive server-side input validation
- Use LIMIT and other SQL controls within queries to prevent mass disclosure of records in case of SQL injection
- For any residual dynamic queries, escape special characters using the specific escape syntax for that interpreter.


## **Example**

- SQL injection
- LDAP injection
- OS command
- Object Relational Mapping (ORM)