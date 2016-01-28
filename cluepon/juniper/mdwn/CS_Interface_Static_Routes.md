---
title: CS Interface Static Routes
permalink: /CS_Interface_Static_Routes/
---

Information
-----------

This script uses a macro applied under the interface configurations to generate static routes, either by manually specifying routes or specifying a prefix-list containing the routes to add. This greatly simplified the management of customers with many static routes which are tied to a specific interface, and addresses the feature request [ER_static_routing_using_prefix_lists](/ER_static_routing_using_prefix_lists "wikilink").

Usage
-----

`interfaces {`
`    xe-1/2/3 {`
`        unit 0 {`
`            family inet {`
`                address 1.2.3.1/30;`
`                apply-macro static {`
`                    10.20.30.0/24 1.2.3.2;`
`                    20.30.40.0/25 1.2.3.2;`
`                }`
`            }`
`        }`
`    }`
`    xe-2/3/0 {`
`        unit 0 {`
`            family inet {`
`                address 2.3.4.1/30;`
`                apply-macro static {`
`                    example-cust-pfxlist 2.3.4.2;`
`                }`
`            }`
`        }`
`    }`
`}`
`policy-options {`
`    prefix-list example-cust-pfxlist {`
`        90.10.20.0/24;`
`        100.10.20.0/24;`
`        110.20.30.0/24;`
`        ...`
`    }`
`}`

Would expand to create the following transient static routes:

`routing-options {`
`    rib inet.0 {`
`        static {`
`            /* Interface-Generated Static Route: xe-1/2/3.0 */`
`            route 10.20.30.0/24 next-hop 1.2.3.2;`
`            /* Interface-Generated Static Route: xe-1/2/3.0 */`
`            route 20.30.40.0/25 next-hop 1.2.3.2;`
`            /* Interface-Generated Static Route: xe-2/3/0.0 */`
`            route 90.10.20.0/24 next-hop 2.3.4.2;`
`            /* Interface-Generated Static Route: xe-2/3/0.0 */`
`            route 100.10.20.0/24 next-hop 2.3.4.2;`
`            /* Interface-Generated Static Route: xe-2/3/0.0 */`
`            route 110.20.30.0/24 next-hop 2.3.4.2;`
`        }`
`    }`
`}`

Bugs
----

In order to specify multiple next-hops for the same prefix, the apply-macro static configuration must be created under multiple interfaces. Under a single interface, only one prefix and next-hop pair can be specified at a time. This may be addressed by future JUNOS enhancements to specify multiple macro values.

Routes can only be added to rib family.0 (i.e. family inet will add routes to inet.0), and no static route attributes other than next-hop can be specified.

ChangeLog
---------

1.0 - Initial Version