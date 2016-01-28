---
title: ER BGP Path-Selection Origin-Ignore
permalink: /ER_BGP_Path-Selection_Origin-Ignore/
---

Problem
-------

JUNOS lacks a way to ignore the BGP Origin attribute in BGP bestpath selection. As it is increasingly common for transit networks to reset the Origin attribute on some or all routes to "I," an operator who wishes to be unaffected by this has no option on JUNOS other than to reset the attribute upon learning routes from all external BGP neighbors.

Solution
--------

JUNOS requires a feature:

`protocols bgp {`
`     path-selection {`
`         origin-ignore;`
`     }`
`} `

Status
------

Not addressed.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")