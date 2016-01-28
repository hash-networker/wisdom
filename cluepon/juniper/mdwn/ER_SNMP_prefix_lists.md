---
title: ER SNMP prefix lists
permalink: /ER_SNMP_prefix_lists/
---

Problem
-------

Currently, [JunOS](/JunOS "wikilink") allows filtering of SNMP access via IP prefixes, but requires that you enter each prefix under the SNMP configuration. The SNMP configuration can not reference prefix-lists from policy-options, which may already have a more complete list of authorized users. This can result in unnecessarily duplicated configuration statements.

For example:

`snmp {`
`    community example {`
`        clients {`
`            1.2.3.4/32;`
`        }`
`    }`
`}`

Solution
--------

Allow the SNMP configuration to reference prefix-lists, as well as manually entered IP prefixes.

For example:

`snmp {`
`    community example {`
`        clients prefix-list authorized_users;`
`    }`
`}`
`policy-options {`
`    prefix-list authorized_users {`
`        1.2.3.4/32;`
`        ....`
`    }`
`}`

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")