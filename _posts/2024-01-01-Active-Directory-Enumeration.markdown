---
layout: post
title:  Active Directory Enumeration
date:   2024-01-01 00:00:00 +0000
categories: internal
---
# Tools / Links
- [https://github.com/BloodHoundAD/SharpHound](https://github.com/BloodHoundAD/SharpHound)
- [https://github.com/fox-it/BloodHound.py](https://github.com/fox-it/BloodHound.py)
- [https://github.com/OPENCYBER-FR/RustHound](https://github.com/OPENCYBER-FR/RustHound)
- [https://hausec.com/2019/09/09/bloodhound-cypher-cheatsheet/](https://hausec.com/2019/09/09/bloodhound-cypher-cheatsheet/)
- [https://github.com/FalconForceTeam/SOAPHound](https://github.com/FalconForceTeam/SOAPHound)

# BloodHound CE Cypher Queries

### Shortest path to Domain Admins from Users
```cypher
MATCH p=shortestPath((n:User)-[*1..]->(g:Group)) WHERE g.objectid ENDS WITH "-512" AND n<>g AND NONE (r in relationships(p) WHERE type(r) = 'LocalToComputer') RETURN p
```

### Shortest path to Domain Admins from Owned
```cypher
MATCH p=shortestPath((n)-[*1..]->(g:Group)) WHERE n.owned AND g.objectid ENDS WITH "-512" AND n<>g AND NONE (r in relationships(p) WHERE type(r) = 'LocalToComputer') RETURN p
```

# Neo4j Console Cypher Queries
Open Neo4j Console at [http://localhost:7474/browser/](http://localhost:7474/browser/)

### Passwords in AD Attributes
```cypher
MATCH (u:User) WHERE NOT u.userpassword IS null RETURN u.name,u.userpassword
MATCH (u:User) WHERE NOT u.unixuserpassword IS null RETURN u.name,u.unixuserpassword
MATCH (u:User) WHERE NOT u.unicodepwd IS null RETURN u.name,u.unicodepwd
MATCH (u:User) WHERE NOT u.msSFU30PasswordIS null RETURN u.name,u.msSFU30PasswordIS
```
