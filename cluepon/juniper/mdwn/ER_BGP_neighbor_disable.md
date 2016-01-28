---
title: ER BGP neighbor disable
permalink: /ER_BGP_neighbor_disable/
---

Problem
-------

JUNOS lacks a way to mark a BGP neighbor as configured but administratively down. The only way to disable a neighbor is to remove it from consideration in the configuration at the parser level, using the generic "deactivate" command:

`protocols bgp {`
`    group example {`
`        inactive: neighbor 1.2.3.4 {`
`            description "Example Neighbor";`
`            remote-as 1234;`
`        }`
`    }`
`}`

Unfortunately this prevents the system from knowing that there is a configured BGP neighbor here at all. It is not displayed in a "show bgp summary", and connection attempts from the remote peer result in system logs appearing to be from unconfigured neighbors, rather than from administratively disabled neighbors. This is counter-intuitive to both users and monitoring software, which has no way to distinguish this from an unconfigured neighbor connection attempt.

This also relates to [ER_Empty_bgp_groups](/ER_Empty_bgp_groups "wikilink"), as the above example would generate warning (or error on code prior to 7.4) because the group has no configured neighbors. The only solution would be to completely deactivate the group as well, which is also counter-intuitive.

Solution
--------

Add a "disable" keyword to the BGP neighbor statement (and secondarily, to the BGP group statements) which places the neighbor in an administratively down state. For example:

`protocols bgp {`
`    group example {`
`        neighbor 1.2.3.4 {`
`            description "Example Neighbor";`
`            remote-as 1234;`
`            disable;`
`        }`
`    }`
`}`

`user@router> show bgp summary `
`...`
`Peer               AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dwn  State...`
`...`
`1.2.3.4          1234      13395      13954       0       0 4d 15:33:31 AdminDown`

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")