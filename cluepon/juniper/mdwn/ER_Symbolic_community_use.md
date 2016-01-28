---
title: ER Symbolic community use
permalink: /ER_Symbolic_community_use/
---

Problem
-------

JUNOS allows routes introduced into the RIB via static routes to have BGP communities applied to them. For example:

`routing-options {`
`    static {`
`        route 1.2.3.0/24 {`
`            discard;`
`            community 1234:4321;`
`        }`
`    }`
`}`

Unfortunately, the use of the "community" keyword in a static route does not allow you to reference "symbolic community names" created under policy-options. This forces users to replicate communities in multiple locations, and lose the benefits of referencing a single symbolically named community.

Solutions
---------

Add support for "named communities" created under policy-options.

`policy-options {`
`    community named_community members 1234:4321;`
`}`
`routing-options {`
`    static {`
`        route 1.2.3.0/24 {`
`            discard;`
`            community named_community;`
`        }`
`    }`
`}`

This is closely related to the problems specified in [ER_Numeric_community_use](/ER_Numeric_community_use "wikilink") and [ER_Numeric_AS_SET_use](/ER_Numeric_AS_SET_use "wikilink").

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")