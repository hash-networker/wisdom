---
title: ER Automatic policer burst sizes
permalink: /ER_Automatic_policer_burst_sizes/
---

Problem
-------

Currently, [JunOS](/JunOS "wikilink") policers require users to manually set the burst-size. For example:

`firewall {`
`    policer example {`
`        if-exceeding {`
`            bandwidth-limit 50m;`
`            burst-size-limit 1m;`
`        }`
`        then discard;`
`    }`
`}`

Juniper has a formula for recommended values based on the the bandwidth limit and properties of the interface (such as MTU), but unfortunately many users are not able to find or accurately calculate these values.

Solution
--------

Add an automatic calculation of the burst-size-limit based on recommended values, but allowing that value to be overridden for specific purposes.

This could be implemented by either not requiring the "burst-size-limit" directive at all:

`firewall {`
`    policer example {`
`        if-exceeding {`
`            bandwidth-limit 50m;`
`        }`
`        then discard;`
`    }`
`}`

Or by adding a new option keyword "auto":

`firewall {`
`    policer example {`
`        if-exceeding {`
`            bandwidth-limit 50m;`
`            burst-size-limit auto;`
`        }`
`        then discard;`
`    }`
`}`

Status
------

Unknown. A temporal policy buffer attribute does exist to set the buffer size in microseconds instead of bytes.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")