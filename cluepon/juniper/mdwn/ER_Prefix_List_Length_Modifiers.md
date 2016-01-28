---
title: ER Prefix List Length Modifiers
permalink: /ER_Prefix_List_Length_Modifiers/
---

Problem
-------

Currently, a [JunOS](/JunOS "wikilink") "prefix-list" is simply a list of prefixes, with no entry "modifiers" that allow matching of more-specific or less-specific routes, such as "upto /24" and "orlonger". This is very different from the way that [Cisco](/Cisco "wikilink") handles prefix-lists. [Cisco](/Cisco "wikilink") allows each prefix-list entry to be tagged with "modifiers" such as "le" and "ge".

Unfortunately, this lack of modifiers prohibits the use of prefix-lists in many prefix filtering roles, such as restricting the announcements of a BGP speaking customer to a known/registered set of prefixes. To achieve the modifier functionality, a unique policy-statement must be written, such as:

`policy-statement CUSTOMER:AS1234 {`
`    term routes {`
`        from {`
`            route-filter 1.2.0.0/16 upto /24;`
`            route-filter 2.3.0.0/19 upto /24;`
`            route-filter 3.4.0.0/18 upto /24;`
`            ...`
`        }`
`        then next policy;`
`    }`
`    then reject;`
`}`

However, prefix-lists do have other valuable uses. One area where they have exceptional functionality is in packet filtering. You may not be able to apply [RPF](/RPF "wikilink") strict-mode on a BGP speaking customer due to possible asynchronous routing, but it may be much more feasible to filter their packets to only source addresses which have been registered for BGP announcement. This would be configured as:

`policy-options {`
`    prefix-list CUSTOMER:AS1234 {`
`        1.2.0.0/16;`
`        2.3.0.0/19;`
`        3.4.0.0/18;`
`        ...`
`    }`
`}`
`firewall {`
`    filter CUSTOMER:AS1234 {`
`        term RPF-allow {`
`            from {`
`                source-prefix-list CUSTOMER:AS1234;`
`            }`
`            then accept;`
`        }`
`        term RPF-deny {`
`            then discard;`
`        }`
`    }`
`}`

Unfortunately, the above solution requires unnecessarily duplicating the customer's prefix list multiple times, in different sections of the configuration.

Potential Solution \#1
----------------------

Allow users to specify prefix modifiers inside the prefix-list, similar to [Cisco](/Cisco "wikilink")'s configuration style.

For example:

`policy-options {`
`    prefix-list CUSTOMER:AS1234 {`
`        1.2.0.0/16 upto /24;`
`        2.3.0.0/19 upto /24;`
`        3.4.0.0/18 upto /24;`
`        ...`
`    }`
`}`

Potential Solution \#2
----------------------

Allow users to apply a prefix-list modifier at the root level of the invocation, which would be applied to all prefixes within the list. This is powerful functionality, though ultimately not as flexible as Potential Solution \#1. An example includes:

`policy-statement CUSTOMER:AS1234 {`
`    term routes {`
`        from {`
`            prefix-list CUSTOMER:AS1234 upto /24;`
`        }`
`        then next policy;`
`    }`
`    then reject;`
`}`

The two solutions can also complement each other nicely, with an option for the invocation modifier to override or not-override the prefix-list defined modifiers. This functionality would prove to be useful when implementing separate policies for normal Internet routing (where upto /24 is normal) and customer driven BGP blackholes (where upto /32 is desired, but only within the local network).

Status
------

A partial implementation of Solution \#2 is implemented in [JunOS](/JunOS "wikilink") 7.4.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")