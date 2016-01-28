---
title: CS BGP TTL Security
permalink: /CS_BGP_TTL_Security/
---

Information
-----------

This script replicates the simple "ttl-security" configuration option provided by Cisco for Generalized TTL Security Mechanism (GTSM) implementation.

Usage
-----

Example:

`protocols {`
`    bgp `
`        group example {`
`            type external;`
`            neighbor 192.168.2.1 {`
`                peer-as 1234;`
`                apply-macro ttl-security;`
`            }`
`        }`
`    }`
`}`

### Configure Firewall Filter

To do the actual hardware drop, you'll need to add a term like this to your loopback lo0 firewall filter to drop non-compliant packets from GTSM neighbors.

`term GTSM-NEIGHBORS {`
`    from {`
`        source-prefix-list {`
`            GTSM-IPV4-NEIGHBORS;`
`        }`
`        ttl-except [ 254 255 ];`
`        protocol tcp;`
`        port 179;`
`    }`
`    then {   `
`        discard;`
`        count DENY-GTSM-IPV4;`
`    }`
`}`

To filter IPv6, add a similar term to your lo0 inet6 filter, but replace the IPV4 reference with IPV6. Juniper prefix-lists cannot mix IPv4 and IPv6, so two lists are required.

Desperate Hack for Missing Feature
----------------------------------

[Native BGP commmand for GTSM/TTL Security](/ER_BGP_TTL_Security_support_enhancements "wikilink")

Bugs
----

This script uses a firewall filter to enforce IP TTL, which only works on T-series and similarly derived hardware (M120/M320/MX/etc). Unfortunately there is no way to match IP TTL in software, so older hardware is not supported.

Using this script as a work-around also breaks the BGP "Fast External Failover" mechanism, where an eBGP session is immediately dropped if the interface link goes down. This can make failure detection take much longer in the event of a outage, potentially up to the configured hold-time (default of 90 secs).

There is also currently no attempt to replicate the "hops" parameter of the Cisco version, as this would require a separate firewall term for every unique TTL value. Implementing this today would be unnecessarily messy, since what you really want is a dedicated firewall filter which you can reference as a subroutine (similar to the policy language), rather than having the script try to insert terms into a firewall filter at potentially arbitrary locations. Since Juniper has so far refused to fix the "bug" which prevents you from creating a chainable firewall filter unless it is actively referenced in a chain, we're not going to attempt that route as a solution either.

Of course at this point you should realize that this is a hack around a blatantly missing feature which should just be implemented in JUNOS directly in the first place, and beg Juniper to do it.

ChangeLog
---------

1.0 - Initial Version