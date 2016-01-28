---
title: ER static routing using prefix lists
permalink: /ER_static_routing_using_prefix_lists/
---

Problem
-------

Often times, providers with non-BGP speaking customers may have a large number of non-contiguous prefixes which must be statically routed to a specific next-hop, then redistributed into internal routing such as IGP or iBGP. All of these static routes from different customers end up piled together under routing-options static, which is difficult to document and manage.

For example:

`interfaces {`
`    ge-1/2/3 {`
`       unit 123 {`
`           description "Example Customer";`
`           family inet {`
`               address 1.2.3.1/30;`
`           }`
`       }`
`       unit 124 {`
`           description "Second Example Customer";`
`           family inet {`
`               address 1.2.3.5/30;`
`           }`
`       }`
`    }`
`}`
`routing-options {`
`    static {`
`        /* These routes are difficult to manage, and identify to a specific customer */`
`        route x.x.x.x/y next-hop 1.2.3.2;`
`        route x.x.x.x/y next-hop 1.2.3.2;`
`        route x.x.x.x/y next-hop 1.2.3.2;`
`        route x.x.x.x/y next-hop 1.2.3.2;`
`        route x.x.x.x/y next-hop 1.2.3.2;`
`        route x.x.x.x/y next-hop 1.2.3.2;`
`        /* You can set comments like this, but then you have to use insert to maintain a grouping */`
`        route x.x.x.x/y next-hop 1.2.3.6;`
`        route x.x.x.x/y next-hop 1.2.3.6;`
`        route x.x.x.x/y next-hop 1.2.3.6;`
`        route x.x.x.x/y next-hop 1.2.3.6;`
`        route x.x.x.x/y next-hop 1.2.3.6;`
`        route x.x.x.x/y next-hop 1.2.3.6;`
`        ...`
`    }`
`}`

Solution
--------

Add an ability to define static routes by referencing a prefix-list for the route lists. This would allow the customer static routes to be more easily managed, allow re-use of the prefix-list for other functions (such as packet filtering), and simplify the configured next-hops.

For example, consolidating the prefixes for the customer into a single prefix-list:

`prefix-list EXAMPLECUSTOMER {`
`    1.1.1.0/24;`
`    1.1.2.0/24;`
`    1.1.7.0/24`
`    1.3.5.0/24;`
`    ...`
`    n.n.n.n/24;`
`}`

### Possible Solution \#1

Add a new keyword which references prefix-lists to create routes.

`routing-options {`
`    static {`
`        route-prefix-list EXAMPLECUSTOMER next-hop 1.2.3.2;`
`    }`
`}`

### Possible Solution \#2

Allow route definitions to reference a prefix-list as well as a single route.

`routing-options {`
`    static {`
`        route EXAMPLECUSTOMER next-hop 1.2.3.2;`
`    }`
`}`

### Possible Solution \#3

A generic policy-statement which can generate static routes.

`routing-options {`
`    static {`
`       generate STATIC-POLICY;`
`    }`
`}`
`policy-options {`
`    policy-statement {`
`       term CUSTOMER {`
`           from {`
`               prefix-list EXAMPLECUSTOMER;`
`           }`
`           then {`
`               next-hop 1.2.3.2;`
`               accept;`
`           }`
`       }`
`       then reject;`
`    }`
`}`

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")