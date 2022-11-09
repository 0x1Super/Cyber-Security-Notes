Tags:Database Fundamentals 
When: [[Daily 2021-09-18]]

##Note:
# What is Database
databases and Structured Query Language (SQL), which databases will use to perform the necessary queries. Web applications utilize back-end databases to store various content and information related to the web application. This can be core web application assets like images and files, web application content like posts and updates, or user data like usernames and passwords.

There are many different types of databases, each of which fits a particular type of use. Traditionally, an application used file-based databases, which was very slow with the increase in size. This lead to the adoption of Database Management Systems (DBMS).

Database Management Systems
A Database Management System (DBMS) is software that helps create, define, host, and manage databases. Various kinds of DBMS were designed over time, such as file-based, Relational DBMS (RDBMS), NoSQL, Graph based, and Key/Value stores.

There are multiple ways to interact with a DBMS, such as command-line tools, graphical interfaces, or even APIs (Application Programming Interfaces). DBMS are used in various banking, finance, and education sectors to record large amounts of data. Some of the essential features of a DBMS include:

Feature	Description
Concurrency	A real-world application might have multiple users interacting with it simultaneously. A DBMS makes sure that these concurrent interactions succeed without corrupting or losing any data.
Consistency	With so many concurrent interactions, the DBMS needs to ensure that the data remains consistent and valid throughout the database.
Security	DBMS provide fine-grained security controls through user authentication and permissions. This will prevent unauthorized viewing or editing of sensitive data.
Reliability	It's easy to backup databases and roll them back to a previous state in case of data loss or a breach.
Structured Query Language	SQL simplifies user interaction with the database with an intuitive syntax supporting various operations.
Architecture
The diagram below details a two-tiered architecture.


Tier I usually consists of client-side applications such as websites or GUI programs. These applications consist of high-level interactions such as user login or commenting. The data from these interactions is passed to Tier II through API calls or other requests.

The second tier is the middleware, which interprets these events and puts them in a form required by the DBMS. The application layer uses specific libraries and drivers based on the type of DBMS to interact with them. The DBMS receives queries from the second tier and performs the requested operations. These operations could include insertion, retrieval, deletion, or updating of data. After processing, the DBMS returns any requested data or error codes in the event of invalid queries.

It is possible to host the application server as well as the DBMS on the same host. However, databases with large amounts of data supporting many users are typically hosted separately to improve performance and scalability.
________
Types of Databases
---
__Relational Databases__

Databases, in general, are categorized into Relational Databases and Non-Relational Databases. Only Relational Databases utilize SQL, while Non-Relational databases utilize a variety of methods for communications.

Relational `DBMS` (RDBMS) are faster  RDBMS store information in tables ![[Pasted image 20210918103407.png]]
  We can link the id from the users table to the user_id in the posts table to easily retrieve the user details for each post, without having to store all user details with each post.

A table can have more than one key, as another column can be used as a key to link with another table. For example, the id column can be used as a key to link the posts table to another table containing comments, each of which belongs to a certain post, and so on.
 `The relationship between tables within a database is called a Schema. `
 This way, by using relational databases, it becomes rapid and easy to retrieve all data about a certain element from all databases
 ______
__Non-relational Databases__

A non-relational database (also called NoSQL database) does not use tables, rows, and columns, nor does it use prime keys, relationships, or schemas. Instead, a NoSQL database stores data using various storage models,
There are 4 common storage models for NoSQL databases:
- Key Value
- Document-Based
- Wide-Column
- Graph     
![[Pasted image 20210918104813.png]]
The most common example of a NoSQL database is ==MongoDB==.
`Non-relational Databases have a different method for injection, known as NoSQL injections. SQL injections are completely different than NoSQL injections. NoSQL injections will be covered in a later module.`

# Commands 
## show 
SHOW DATABASES;
SHOW TABLES;
SHOW FIELDS FROM table / DESCRIBE table;
## Select 
SELECT * FROM table;
SELECT * FROM table1, table2;
SELECT field1, field2 FROM table1, table2;

# use
USE DatabaseName;
DROP DATABASE DatabaseName;