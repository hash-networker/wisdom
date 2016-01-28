---
title: ER BGP Neighbor prefix list
permalink: /ER_BGP_Neighbor_prefix_list/
---

Problem
-------

Currently, in order to utilize a prefix-list to filter BGP routes, a user must create both a prefix-list and a policy-statement which references that prefix-list, for example:

`policy-options {`
`    prefix-list CUSTOMER:AS1234 {`
`        1.2.3.0/24;`
`        2.3.0.0/16;`
`        3.4.0.0/19;`
`        ...`
`    }`
`    policy-statement CUSTOMER:AS1234 {`
`        term routes {`
`            from route-list-filter CUSTOMER:AS1234 orlonger;`
`            then next policy;`
`        }`
`        then reject;`
`    }`
`}`
`protocol bgp {`
`    group customer {`
`        neighbor x.x.x.x {`
`            peer-as 1234;`
`            import [ CUSTOMER:AS1234 ... ];`
`            ...`
`        }`
`    }`
`}`

In addition to requiring an often unnecessary policy-statement, if this is done on an export policy it can unnecessarily break BGP neighbor grouping (update replication) by creating a unique export policy where none is necessary.

Solution
--------

Implement a per BGP Neighbor prefix-list, which is applied before the import/export policy-statements to remove routes which will always be explicitly filtered.

For example:

`policy-options {`
`    prefix-list CUSTOMER:AS1234 {`
`        1.2.3.0/24;`
`        2.3.0.0/16;`
`        3.4.0.0/19;`
`        ...`
`    }`
`}`
`protocol bgp {`
`    group customer {`
`        neighbor x.x.x.x {`
`            peer-as 1234;`
`            import [ ... ];`
`            prefix-list input CUSTOMER:AS1234 orlonger;`
`            ...`
`        }`
`    }`
`}`

This has several advantages. In addition to eliminating an unnecessary policy-statement and making the configuration more manageable, the routes which are filtered by this prefix-list can have a "keep none" applied to reduce memory usage. This is also a viable solution for the problems illustrated by [ER BGP Prefix limit enhancements](/ER_BGP_Prefix_limit_enhancements "wikilink"), and is more consistent with Cisco behavior which many users may be more familiar with.

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")