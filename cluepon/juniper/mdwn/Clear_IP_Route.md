---
title: Clear IP Route
permalink: /Clear_IP_Route/
---

Clear a single IP route from the forwarding table

`> clear route forwarding-table [route]`

*IOS Equivalent: clear ip route \[route\]*

This statement is quite dangerous because it alters sync between Routing Table and Forwarding Table: so be carefull and if you have to perform a "clear route forwarding-table <prefix>" for any reason, you should perform a "disable/enable" of "forwarding-table load-balancing" to take back the two tables in sync.