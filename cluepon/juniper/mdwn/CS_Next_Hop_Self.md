---
title: CS Next Hop Self
permalink: /CS_Next_Hop_Self/
---

Information
-----------

In JUNOS, when you configure a policy with a statement of "next-hop self", the value of "self" is taken from the local IP address of the BGP session. On Cisco you can manually specify an interface to use for the BGP next-hops, i.e. "neighbor x.x.x.x update-source lo0", but JUNOS lacks this functionality. This presents a problem when you want to carry more than one address-family in a single BGP session, for example when using a single IPv4 IBGP session to carry both inet and inet6 NLRIs. In the example above, if you set next-hop self on an inet6 route, rather than use a valid IPv6 address JUNOS will use "::x.x.x.x" derived from the IPv4 address.

This script builds a policy-statement NHS which correctly sets the next-hops for both family inet and inet6, using the values configured in your lo0 interface.

Usage
-----

With an existing configuration of:

`interface {`
`    lo0 {`
`        unit 0 {`
`            family inet {`
`                address 1.2.3.4/32;`
`            }`
`            family inet6 {`
`                address 2001:1234::5678/128;`
`            }`
`        }`
`    }`
`}`

This script will generate the following policy-statement:

`policy-statement NHS {`
`    term inet {`
`        from family inet;`
`        then next-hop 1.2.3.4;`
`    }`
`    term inet6 {`
`        from family inet6;`
`        then next-hop 2001:1234::5678;`
`    }`
`}`

You would then reference this in other policy-statements to set next-hop self, rather than using the built-in command:

`policy-statement example {`
`    ...`
`    term SET-NHS {`
`        from policy NHS;`
`    }`
`}`

Desperate Hack for Missing Feature
----------------------------------

[Command to specify an interface update-source for BGP next-hops](/ER_BGP_Update_Source_command "wikilink")

Bugs
----

This script does nothing special to handle multiple loopback addresses or multiple addresses configured under a single loopback. The first value of each is used. Within a single loopback, you can select which address to use by configuring it "primary".

ChangeLog
---------

1.0 - Initial Version