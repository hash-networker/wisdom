---
title: Load Balancing
permalink: /Load_Balancing/
---

Juniper routers offer three types of load-balancing options:

-   Aggregated Interfaces (Layer 2)
-   Equal-Cost Multipath (ECMP) (Layer 3)
-   BGP Multipath (Layer 3)

Layer 2 Load Balancing
----------------------

### Aggregated Interfaces

Juniper Aggregated Interfaces (also called *AE* for Aggregated Ethernet, and *AS* for Aggregated SONET) are a layer 2 mechanism for load-balancing across multiple interfaces between two devices. This is essentially the same type of mechanism as Cisco EtherChannel, later standardized into IEEE 802.3ad, and was adapted by Juniper as a proprietary extension to achieve a similar behavior over SONET interfaces.

Because this is a layer 2 load-balancing mechanism, all of the individual component links must be between the same two devices on each end. Juniper supports a non-signaled (static) configuration for Ethernet and SONET, as well as the 802.3ad standardized LACP protocol for negotiation over Ethernet links. One caveat is that on first generation [Martini](/Martini "wikilink") based platforms, doing LACP + 802.1q tagging requires an FPC-E or later.

Layer 3 Load Balancing
----------------------

In order to understand how ECMP is configured, first consider the following structure of a routing table entry:

`route (prefix)`
`    path 1`
`        next-hop: a.a.a.a`
`        next-hop: b.b.b.b`
`    path 2`
`        next-hop: c.c.c.c`
`        next-hop: d.d.d.d`
`    path n`
`        next-hop: n.n.n.n`

For any given route there may be multiple paths to the destination (for example, two transit providers offering two individual paths via BGP for the same prefix). By default, only one of these paths is active, and in a "show route" the Juniper router will indicate the reason that any particular path is inactive. Within a given path, there may also be multiple physical or logical (indirect) next-hops for forwarding purposes.

### Equal-Cost Multipath (ECMP)

Equal-Cost Multipath (ECMP) is a layer 3 mechanism for achieving load balancing for a path with multiple equal cost next-hops. By default, in the event of equal-cost next-hops Juniper will randomly pick one next-hop per route to install for hardware forwarding. There is also an option that allows multiple next-hops to be installed into hardware, known as "per-packet load balancing".

The term "per-packet load balancing" as used by Juniper is actually misleading in two ways. First, only the first generation [Internet Processor I](/Internet_Processor_I "wikilink") ASIC based routers were capable of true per-packet load-balancing, which is generally not a desirable behavior. All other Juniper routers actually implement a "per-flow" load-balancing, which prevents packet reordering, but the configuration element is still called "per-packet" for compatibility. Second, this option doesn't actually have a direct relationship to "how" the packets are forwarded at all, it simply enables the installation of multiple next-hops for the same route.

An example configuration, which enabled per-packet load balancing for every prefix at the time it is exported to the forwarding table, is provided below:

`policy-options {`
`    policy-statement your-name-here {`
`        then {`
`            load-balance per-packet;`
`        }`
`    }`
`}`
`routing-options {`
`    forwarding-table {`
`        export [ your-name-here ];`
`    }`
`}`

### BGP Multipath

In addition to load-balancing between equal-cost next-hops, BGP routes can be load-balanced between multiple paths as well. Insert example of bgp multipath configuration here.

Packet Hashing
--------------

Regardless of whether you load-balance via a layer 2 or layer 3 method, the hashing method used for actual packet forwarding can be configured under the "forwarding-options hash-key" hierarchy.

### IP Hashing

The forwarding-based load balancing is the process used to select from multiple next-hops. By default, the inet forwarding process uses a hash algorithm based on layer-3 information only. This includes:

-   Source IP Address
-   Destination IP Address
-   Protocol

It is also possible to configure the enter device to use layer-4 information. This adds the following information to the hashing algorithm:

-   Source port number
-   Destination port number
-   Incoming interface index

An example configuration using both layer-3 and layer-4 information for path selection is:

`forwarding-options {`
`    hash-key {`
`        family inet {`
`            layer-3;`
`            layer-4;`
`        }`
`    }`
`}`

One key consideration of adding layer-4 information is that a potentially different hash result for different source/destination ports may cause the output of a "traceroute" being performed through the router to become jumbled, potentially confusing novice users. The implementation of a traditional UDP-based traceroute uses an incrementing source and destination port for each probe, with multiple probes per hop. This may result in each hop returning a different IP address per probe.

An example traceroute through routers using layer-4 hashing, cleaned up for extra readability:

`5  p16-1-2-2.r21.nycmny01.us.bb.verio.net (129.250.4.26)  8.582 ms  7.536 ms`
`   p16-4-0-0.r00.chcgil06.us.bb.verio.net (129.250.5.102)  26.229 ms`
`6  p16-2-0-0.r21.sttlwa01.us.bb.verio.net (129.250.2.180)  71.146 ms `
`   p16-1-1-3.r20.sttlwa01.us.bb.verio.net (129.250.2.6)  66.559 ms  66.588 ms`
`7  xe-0-2-0.r20.sttlwa01.us.bb.verio.net (129.250.4.16)  71.166 ms `
`   xe-3-1.r00.sttlwa01.us.bb.verio.net (129.250.2.205)  66.824 ms `
`   xe-0-2-0.r20.sttlwa01.us.bb.verio.net (129.250.4.16)  71.677 ms`

### MPLS Hashing

### CCC Hashing

### Other Hashing

### Hidden Hash Configuration Summary

NOTE: Configuration listed below in ***bold italics*** is hidden; use at your own risk.

`forwarding-options {`
`    hash-key {`
`        family inet {`
`            layer-3 {`
`                `***`destination-address`***`;`
`                `***`incoming-interface-index`***`;`
`                `***`protocol`***`;`
`                `***`source-address`***`;`
`            }`
`            layer-4 {`
`                `***`destination-port`***`;`
`                `***`source-port`***`;`
`                `***`type-of-service`***`;`
`            }`
`        }`
`        family mpls {`
`            `***`label-1`***`;`
`            `***`label-2`***`;`
`            `***`no-interface-index`***`;`
`            `***`payload`***` {`
`                `***`ip`***`;`
`            }`
`        }`
`    }`
`}`