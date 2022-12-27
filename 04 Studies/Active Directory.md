
# Physical Active Directory

## What is domain controller

- A domain controller is a server with AD DS (Active Directory Domain Services) server role installed that has specifically been promoted to a domain controller 

## What it does

### Domain controllers:
- Host a copy of the AD DS directory store
- Provide authentication and authorization services
- Replicate updates to other domain controllers in the domain and forest
- Allow administrative access to manage user accounts and network resources 

## AD DS Data Store

- The AD DS data store contains the database files and processes that store and manage directory information for users, services, and applications

### The AD DS data store:

- Consists of the Ntds.dit file
- Is stored by default in %SystemRoot%\ NTDS folder on all domain controllers
- Is accessible only through the domain controller processes and protocols 


# Logical AD Components


### The AD DS Schema:
- Defines every type of object that can be stored in the directory 
- Enforces rules regarding object creation and configuration 

| Obbject Types | Function | Exmaples|
|----|---|---|
|Class Object | What objects can be created in the directory | User / Computer |
| Attribute Object | Information that can be attached to an object | Display name |

## Domains 

![[Pasted image 20221222161115.png]]
### Domains:
- An administrative boundary for applying policies to group of objects
- A replication boundary for replicating data between domain controllers 
- An authentication and authorization boundary that provides a way to limit the scope of access to resources 

## Trees
![[Pasted image 20221222161438.png]]

- Trees are the child of top level domain 

### All domains in the tree:
- Share a contiguous namespace with the parent domain \*.contoso.com 
- Can have additional child domains \*.\*.contoso.com
- By default create a two-way transitive trust with other domains 

## Forest
![[Pasted image 20221222161815.png]]

- Share a common schema
- Share a common configuration partition
- Share a common global catalog to enable searching
- Enable trust between all domains in the forest 
- Share the enterprise Admins and Schema Admins groups

## Organizational Units (Ous)

![[Pasted image 20221222161957.png]]

Containers for users, groups and etc.
- Represent your organization hierarchically and logically 
- Manage a collection of objects in a consistent way
- Delegate permissions to administer groups of objects
- Apply policies


## Trusts

- Trusts provide a mechanism for users to gain access to resources in another domain

![[Pasted image 20221222171807.png]]

- All domains in a forest trust all other domains in the forest
- Trusts can extend outside the forest

## Objects

![[Pasted image 20221222171907.png]]
