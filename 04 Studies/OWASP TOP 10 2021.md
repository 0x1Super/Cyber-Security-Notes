
# `1-  Broken access Control`
Access control  is the application of constraints on who (or what) can perform attempted actions or access resources that they have requested. In the context of web applications

## **Cause**

- Broken access control vulnerabilities exist when a user can in fact access some resource or perform some action that they are not supposed to be able to access.


## **Examples**:

- Violation of the principle of least privilege or deny by default
- Bypassing access control checks by modifying the URL
- Permitting viewing or editing someone else's account
- Accessing API with missing access controls for POST, PUT and DELETE.
- Elevation of privilege, Visiting authorized web pages that can only be accessed by authorized users
- Metadata manipulation, such as replaying or tampering with a JSON Web Token (JWT) access control token, or a cookie or hidden field manipulated to elevate privileges or abusing JWT invalidation.

## **Prevention**:

- Deny by default 
- Access control mechanism  
- Record ownership 
- Log access control failure
- Rate limit APIs
- Least Privileges 



# `2- Cryptographic Failures`
## **Description**

The first thing is to determine the protection needs of data in transit and at rest. For example, passwords, credit card numbers, health records, personal information, and business secrets require extra protection

## **Cause:**
- encryption not enforced
- Using weak  cryptographic algorithms or protocols used either by default or in older code
- Data transmitted in clear text
- server certificate and the trust chain  not validated
- passwords being used as cryptographic keys in absence of a password base key derivation function?
- deprecated hash functions such as MD5 or SHA1 in use, or are non-cryptographic hash functions used when cryptographic hash functions 
- Weak Cipher suite 


## **Examples**:

- Downgrade attacks
- Cookie Theft 
- Sniffing/ MITM attacks
- Injection attacks

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



# `3- Injection`
## **Description**


Almost any source of data can be an injection vector, environment variables, parameters, external and internal web services, and all types of users. Injection flaws occur when an attacker can send hostile data to an interpreter.

## **Cause**

- User-supplied data is not validated, filtered, or sanitized by the application.
- Dynamic queries or non-parameterized calls without context-aware escaping are used directly in the interpreter
- Hostile data is used within object-relational mapping (ORM) search parameters to extract additional, sensitive records


## **Example**

- SQL injection
- LDAP injection
- OS command
- Object Relational Mapping (ORM)


## **Prevention**

- The preferred option is to use a safe API, which avoids using the interpreter entirely, provides a parameterized interface
- Use positive server-side input validations 
- Use LIMIT and other SQL controls within queries to prevent mass disclosure of records in case of SQL injection
- For any residual dynamic queries, escape special characters using the specific escape syntax for that interpreter.


# 4- Insecure Design


## Description

Insecure design is a broad category representing different weaknesses, expressed as “missing or ineffective control design.” Insecure design is not the source for all other Top 10 risk categories. There is a difference between insecure design and insecure implementation. We differentiate between design flaws and implementation defects for a reason, they have different root causes and remediation. A secure design can still have implementation defects leading to vulnerabilities that may be exploited. An insecure design cannot be fixed by a perfect implementation as by definition, needed security controls were never created to defend against specific attacks. One of the factors that contribute to insecure design is the lack of business risk profiling inherent in the software or system being developed, and thus the failure to determine what level of security design is required.

## How to Prevent

-   Establish and use a secure development lifecycle with AppSec professionals to help evaluate and design security and privacy-related controls
    
-   Establish and use a library of secure design patterns or paved road ready to use components
    
-   Use threat modeling for critical authentication, access control, business logic, and key flows
    
-   Integrate security language and controls into user stories
    
-   Integrate plausibility checks at each tier of your application (from frontend to backend)
    
-   Write unit and integration tests to validate that all critical flows are resistant to the threat model. Compile use-cases _and_ misuse-cases for each tier of your application.
    
-   Segregate tier layers on the system and network layers depending on the exposure and protection needs
    
-   Segregate tenants robustly by design throughout all tiers
    
-   Limit resource consumption by user or service















---
Tags: #OWASP #OWASP_checklist
Resources: 
[OWASP top 10](https://owasp.org/www-project-top-ten/)
[Checklist](https://github.com/tanprathan/OWASP-Testing-Checklist)
[Checklist pdf](https://owasp.org/www-project-web-security-testing-guide/assets/archive/OWASP_Testing_Guide_v4.pdf)
[OWASP top 10 pdf](https://owasp.org/www-chapter-minneapolis-st-paul/download/20211216_OWASP-MSP_OWASP_Top_Ten_2021.pdf?raw=true) 