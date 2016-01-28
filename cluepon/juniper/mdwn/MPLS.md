---
title: MPLS
permalink: /MPLS/
---

The folowing examples shows how to build minimal MPLS configurations.

Interface activation
--------------------

The MPLS family requires to be activated on all the interfaces using MPLS traffic. Enter the *\[edit interfaces\]* mode to start configuring MPLS.

` user@juniper> edit`
` user@juniper# edit interfaces`
` `
` [edit interfaces]`
` user@juniper# set ge-0/0/0 unit 0 `**`family` `mpls`**
` `
` [edit interfaces]`
` user@juniper# set fe-0/0/1 unit 0 `**`family` `mpls`**
` `
` [edit interfaces]`
` user@juniper# show`
`     fe-0/0/1 {`
`         unit 0 {`
`             family inet {`
`                 address 192.168.0.1/24;`
`             }`
`             `**`family` `mpls;`**
`         }`
`     }`
`     ge-0/0/0 {`
`         unit 0 {`
`             family inet {`
`                 address 192.168.1.1/24;`
`             }`
`             `**`family` `mpls;`**
`         }`
`     }`
` `
` [edit interfaces]`
` user@juniper# top`

Protocol activation
-------------------

Before MPLS can be implented in your network it must be explicitly be enabled under the *\[edit protocols\]* section.

` [edit]`
` user@juniper# edit protocols`
` `
` [edit protocols]`
` user@juniper# `**`set` `mpls` `interface` `all`**
` `
` [edit protocols]`
` user@juniper# show`
`     mpls {`
`         interfaces all;`
`     }`
` `
` [edit protocols]`
` user@juniper# commit`

You may need to enable LDP on your interfaces to allow for neighbor discovery, under *\[edit protocols\]\]*.

` [edit protocols]`
` user@juniper# `**`set` `ldp` `interface` `all`**

Default labels
--------------

The example shows the default MPLS routing table. There are two labels, *0* and *1*, created by [JunOS](/JunOS "wikilink"). Label *0* is the IPv4 explicit *NULL*, and *1* is the router alert label.

` [edit]`
` user@juniper# `**`run`**` show route table mpls.0`
` `
` mpls.0: 2 destinations, 2 routers )2 active, 0 holddown, 0 hidden)`
` + = Active Route, - = Last Active, * = Both`
` `
` 0                     *[MPLS/0] 00:05:12, metric 1`
`                         Receive`
` 1                     *[MPLS/1] 00:05:12, metric 1`
`                         Receive`

Static LSP *(Labeled Switched Path)* configuration
--------------------------------------------------

[Category:Unsupported activities](/Category:Unsupported_activities "wikilink")

The folowing example will show a minimum configuration for a static LSP. We will create an MPLS connection between InterXion Paris and InterXion Brussels. Normall, an IGP *(Interior Gateway Protocol)* like OSPF *(Open Shortest Path First)* or IS-IS *(Intermediate System-to-Intermediate System)* would determine the path between Brussels and Paris. There is an label-switching router in Luxembourg to swap labels.

The first output shows Paris' routing table prior to the configuration of a static LSP. Notice that the prefered path is the IGP (OSPF)

` user@paris> show route 192.168.0.1`
` `
`   inet.0: 10 destinations, 10 routes (9 active, 0 holddown, 1 hidden)`
`   + = Active Route, - = Last Active, * = Both`
` `
`   192.168.0.1/32      *[OSPF/10] 00:00:15, metric 20`
`                         > to 192.168.1.1 via fxp1.0`

We will configure the static LSP from Paris through Luxembourg and reach the destination in Brussels. The example show the configuration used for Parix. In addition to adding the *family mpls* to all the required interfaces it's needed to add the *static-path* attribute to the configuration. This inserts a static route into the routing table for the address *192.168.0.1/32*. Label 50 will be pushed on the packed destinated for *192.168.0.1/32*. when they leave the LSR (Label Switching Router) for the next-hop 192.168.1.1

` [edit protocols mpls]`
` user@paris# `**`set` `static-path` `inet` `192.168.0.1` `next-hop` `192.168.1.1` `push` `50`**
` `
` [edit protocols mpls]`
` user@paris# show`
`     static-path inet {`
`         192.168.0.1/32 {`
`             next-hop 192.168.1.1;`
`             push 50;`
`         }`
`         interface all;`
`     }`

Since Luxembourg is a transit router, the only function it will serve is to swap the label. Label 50 is received from Parix and swapped with label 0, which will be sent to Brussels. When Brussels received the label 0 it will know to pop the label and route the packed like normal Ipv4 traffic.

` [edit protocols mpls]`
` user@luxembourg# `**`set` `interface` `ge-0/0/0` `label-map` `50` `next-hop` `192.168.3.1` `swap` `0`**
` `
` [edit protocols mpls]`
` user@luxembourg# show`
`     interface all;`
`     interface ge-0/0/0.0 {`
`         label-map 50 {`
`             next-hop 192.168.3.1;`
`             swap 0;`
`          }`
`     }`

We now have a fully working static LSP path. When we look at the route again, we shall see the new labeled path.

` user@paris> show route 192.168.0.1`
` `
`   inet.0: `**`11`**` destinations, `**`11`**` routes (`**`10`**` active, 0 holddown, 1 hidden)`
`   + = Active Route, - = Last Active, * = Both`
` `
`   192.168.0.1/32      `**`*[Static/5]` `19:52:42`**
`                         > to 192.168.1.1 via fxp1.0, `**`Push` `50`**
`                       *[OSPF/10] 19:52:43, metric 20`
`                         > to 192.168.1.1 via fxp1.0`