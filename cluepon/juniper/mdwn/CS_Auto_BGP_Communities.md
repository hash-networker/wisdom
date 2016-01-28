---
title: CS Auto BGP Communities
permalink: /CS_Auto_BGP_Communities/
---

Information
-----------

See <http://www.nanog.org/meetings/nanog40/presentations/BGPcommunities.pdf>

Usage
-----

`system {`
`    location {`
`        apply-macro bgp {`
`            continent 1;`
`            region 4;`
`            city 12;`
`        }`
`    }`
`}`

Bugs
----

ChangeLog
---------

1.0 - Initial Version