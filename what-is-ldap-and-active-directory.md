# 3 What is LDAP and Active Directory ? How LDAP works and what is the structure of LDAP/AD?
- Talented Developer
- Jun 24, 2019
- [https://www.youtube.com/watch?v=QyhNaY5O468](https://www.youtube.com/watch?v=QyhNaY5O468)
- Completed Apr 17, 2020
---

### AD - Active Directory
- A directory service database used to provide authentication as well as group and user management.
- Stores the policies used for authorizations.
- It authenticates and authorizes users and their computers.

### LDAP - Lightweith Directory Access Protocol
- An open cross-platform client-server protocol used for directory service authentication.
- It is used to access and manage the directory information.
- Runs over TCP/IP protocol.
- **LDAP** is a way of speaking to the **Active Directory**.
  
### How LDAP works
```
+----------+      +-------------+       +-------------------------+
| Username |----->|             |------>|                         |
| Password |      | API Service |<------| LDAP / Active Directory |
+----------+      +-------------+       +-------------------------+
                        |
                        V
                  +-------------+
                  |             |            
                  | Application |
                  +-------------+
```

### Why LDAP?
- You couldn't possibly create a policy for every single user and resource combination.
- Create security policies within the domain controller.

### LDAP Structure
```
                              +-------+
                              | ROOT  |
                              +-------+
                                  |
                                  V
                         +--------+-----------+
Domain Controller        | dc=example, dc=com |
                         +--------+-----------+
                                  |
                           +------+--------+
                           |               |
                           V               V
                    +----------+     +----------+ 
Organization Unit   | ou=user  |     | ou=group |
                    +----------+     +----------+ 
                         |
                         V
                    +----------+                         
Common Name         | cn=John  |
                    +----------+                         
```
- The Domain Component represent sthe top of the LDAP tree that uses DNS to define the namespace.
- The Organization Unit acts as a container that holds all types of objects that provide a structure to the LDAP.
- You can add security policies to **Organization Groups**.
- One person can belong to different groups.
- **Group names must be unique.**

### Example
```
                                 +-------+
                                 |  ABC  | Organization
                                 +-------+
                                    |
                                    V
          +-------------------------+--+----------------------------+
          |                            |                            |
          V                            V                            V
+------------------+         +------------------+         +------------------+
|     Developer    |         |     Business     |         |     Finance      |  Organization
+------------------+         +------------------+         +------------------+  Unit
          |                    |       |       |                   |
          V                    V       V       V                   V
  +-------------+           +-----+ +-----+ +-----+         +--------------+ 
  |   Backend   |           |  O  | |  O  | |  O  |         |   Accounts   |  Organization
  +-------+-----+           +-----+ +-----+ +-----+         +--------------+  Unit
          |                                                        |
          V                                                        V
       +-----+                                                  +-----+
       |  O  |  Attribute                                       |  O  |  Attribute
       +-----+                                                  +-----+
         |
         V
  +---------------+ +----------------------+
  | Name=John     | | cn=john, ou=Backend, |
  | Address=India | | ou=Developer, o=ABC  |
  | Phone=0986    | +----------------------+   
  +---------------+
```

### You can create a **super admin** in a group.
- So a single person can have control over a group.

### Terms
- **o** - organization name
- **ou** - organization unit
- **cn** - common name
- **sn** - surname
- **dn** - distinguish name
- **User** - inetOrgPerson  [Object]
- **User** - groupsOfUniqueName  [Object]

### Authentication Types
1. **Simple** 
   - Enter username and password.
   - Authenticate the user if they are valid.
2. **SASL**
   - The client and server negotiate an authentication mechanism.