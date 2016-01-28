---
title: CS Dual RE Hostname Manager
permalink: /CS_Dual_RE_Hostname_Manager/
---

Information
-----------

This script automatically configures the groups necessary to set the hostname to "re\#.hostname", where re\# is the routing engine number currently being used.

Usage
-----

`system {`
`    host-name rtr1.example1;`
`}`

Will automatically configure the host-name by building the configurations for the necessary groups:

`groups {`
`    re0 {`
`        system {`
`            host-name re0.rtr1.example1;`
`        }`
`    }`
`    re1 {`
`        system {`
`            host-name re1.rtr1.example1;`
`        }`
`    }`
`}`
`apply-groups [ re0 re1 ];`

ChangeLog
---------

1.0 - Initial Version