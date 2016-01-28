---
title: ER BGP Prefix limit enhancements
permalink: /ER_BGP_Prefix_limit_enhancements/
---

Problem
-------

Currently, JUNOS supports a BGP prefix-limit command which limits the number of routes that can be received on a session. When the number of routes exceeds this configured limit, the session can be torn down until the remote side fixes their misconfiguration.

`protocol bgp {`
`    group customer {`
`        family inet {`
`            unicast {`
`                prefix-limit {`
`                    maximum 500;`
`                    teardown 80 idle-timeout 5;`
`                }`
`            }`
`        }`
`    }`
`}`

However, this limit is applied to a session by counting all prefixes received, even prefixes which have been explicitly rejected by a policy-statement and can never be considered as feasible paths. Cisco on the other hand implements it prefix-limit based on the number of routes which pass filtering mechanisms such as prefix-list and as-path, and are eligible for selection as best path.

The reality of the Internet is that customers, peers, etc do make configuration mistakes from time to time. A common example is a BGP speaking customer who is not intimately familiar with their router configurations accidentally leaking the full prefixes of one of their upstream providers to another non-customer network. Most sensible networks already take precautions to limit the prefixes that can be announced, via a prefix-list filter or as-path filter. Unfortunately, even if a leaking customer or peer is already covered by a more restrictive filter which prevents the leaking of inappropriate routes, Juniper routers will still drop the BGP session, preventing the use of a prefix-limit as a mechanism of last resort to catch configuration errors.

A commonly given reason for why Juniper implements this behavior is to protect the router against memory exhaustion due to an errored BGP session which for example deaggregates the Internet into /24s. While catching this condition is a worthy goal, it does not align with the much more common practical use of a prefix-limit, as a policy control to prevent leaking of routes. Dropping the session unnecessarily is typically not a desired behavior, and is disruptive to a customer or peer session which otherwise could have continued to operate normally due to the presence of more restrictive filters.

Solution
--------

Implement a second type of prefix-limit variable which only counts prefixes eligible for installation as best path. The first prefix-limit can continue to act as a filter to protect the router from unchecked memory consumption, and the second prefix-limit can be used to provide policy protection against leaked routes without triggering unnecessary BGP shutdowns.

For example:

`protocol bgp {`
`    group customer {`
`        family inet {`
`            unicast {`
`                prefix-limit {`
`                    maximum 250000;`
`                    maximum-feasible 500;`
`                    teardown 80 idle-timeout 5;`
`                }`
`            }`
`        }`
`    }`
`}`

This would be backwards compatible with existing configurations as well.

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")