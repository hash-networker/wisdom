---
title: ER BGP Default originate
permalink: /ER_BGP_Default_originate/
---

Problem
-------

Currently, it is extremely difficult to originate a default route via BGP. Using a policy-statement is overly complicated, and requires that a default route exist on the router. Even if a default route does exist, many users mark it "no-readvertise" so that it is not accidentally matched by other announcement/redistribution policies. This means that in order to advertise a default route to a specific BGP neighbor, you must have a static route capable of being readvertised, and make explicit exceptions for it in all other announcement/redistribution policies.

An example of the configuration statements necessary to support originating a default route via BGP is:

`routing-options {`
`    static {`
`        route 0.0.0.0/0 discard;`
`    }`
`}`
`policy-options {`
`    policy-statement igp-redistribute {`
`        term exclude-default {`
`            from {`
`                route-filter 0.0.0.0/0 exact;`
`            }`
`            then reject;`
`        }`
`        ...`
`    }`
`    ...potentially other redistribution policies...`
`    policy-statement default-originate {`
`        term default {`
`            from {`
`                route-filter 0.0.0.0/0 exact;`
`            }`
`            then accept;`
`        }`
`        then next policy;`
`    }`
`}`
`protocols {`
`    bgp {`
`        group example {`
`            neighbor 1.2.3.4 {`
`                remote-as 1234;`
`                export [ bgp-originate other-export-policies ... ];`
`            }`
`        }`
`    }`
`}`

Obviously, this is both complicated and more likely to result in accidental misconfigurations.

Solution
--------

Add a specific BGP neighbor flag that injects a default route, regardless of the existence of the default route on the router. This is also the way that [Cisco](/Cisco "wikilink") and most other platforms handle this problem, which reduces the Juniper learning curve for other users.

Minimal example:

`protocols {`
`    bgp {`
`        group example {`
`            neighbor 1.2.3.4 {`
`                remote-as 1234;`
`                default-originate;`
`            }`
`        }`
`    }`
`}`

Potentially more complex example:

`protocols {`
`    bgp {`
`        group example {`
`            neighbor 1.2.3.4 {`
`                remote-as 1234;`
`                default-originate {`
`                    metric 50;`
`                    community 4321:1234;`
`                    other BGP parameters;`
`                }`
`            }`
`        }`
`    }`
`}`

Another potential solution:

`policy-options {`
`    policy-statement default-originate-params {`
`        then {`
`            metric 50;`
`            community 4321:1234;`
`            other BGP parameters;`
`        }`
`    }`
`}`
`protocols {`
`    bgp {`
`        group example {`
`            neighbor 1.2.3.4 {`
`                remote-as 1234;`
`                default-originate default-originate-params;`
`            }`
`        }`
`    }`
`}`

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")