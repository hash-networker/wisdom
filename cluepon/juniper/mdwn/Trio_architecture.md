---
title: Trio architecture
permalink: /Trio_architecture/
---

The **Trio architecture** announced in November 2009, powering the [MX80](/MX80 "wikilink"), [MPC](/MPC "wikilink")1 and MPC2 cards, and the 16-port SFP+ MPC on the [MX-series](/MX-series "wikilink") routers.

Trio Chipset
------------

The Trio Chipset is composed of 4 individual ASICs, each of which is responsible for different functions within a single [PFE](/PFE "wikilink"). Every Trio PFE will contain and MQ and LU chip, while the QX and IX chips are optional.

### MQ-Chip

The MQ-chip controls the packet memory, connects to the WAN interfaces and the chassis fabric, and is responsible for sending the first 256 bytes of each packet to the LU-chip for processing. The MQ-chip also performs port-based QoS, and manages scheduling and shaping of the ports and queues. Each MQ-chip has four 10GE MACs, that can be used to directly drive four 10GE PHYs.

Each MQ-chip is capable of handling approximately 70Gbps of total unidirectional traffic, though the exact performance depends on the packet size. This bandwidth is shared between the fabric and WAN interfaces, so with 1:1 bidirectional traffic each MQ-chip is capable of handling 35Gbps. If the traffic is not bidirectional, the MQ-chip is capable of achieving line-rate 40Gbps per PFE (e.g. 40Gbps in, 30Gbps out).

### LU-Chip

The LU-chip implements all of the lookup and packet processing features, including routing, MAC, and MPLS label lookups. It is also responsible for implementing firewalls, multi-field classifiers, policers, maintaining accounting and interface statistics, and performing packet header rewrites and layer 2 encapsulations.

Each LU-chip is capable of processing approximately 55Mpps, though the exact performance depends on the features enabled.

Some Trio PFE also support external TCAM, which the LU-chip can use to offer accelerated lookup processing of certain features.

### IX-Chip

The IX-chip implements support for 1GE interfaces, and talks to the MQ-chip for further processing. Each IX-chip supports up to (24) 10/100/1000 GigE MACs and (2) 10GE MACs, as well as pre-classification of incoming packets.

### QX-Chip

The QX-chip implements fine-grained hierarchical queueing. If HQoS is supported the QX-chip handles all queueing and scheduling functions. Otherwise, the MQ-chip handles port-level queueing and scheduling.

Trio Architecture
-----------------

<graphviz> graph triooverall { fontsize=10

` "LU" [shape=box];`
` "QX" [shape=box];`
` "MQ" [shape=box];`
` "RLDRAM" [shape=box];`
` "DDR3" [shape=box];`
` "LU" -- "MQ" [color=blue];`
` "QX" -- "MQ" [color=blue];`
` "Fabric" -- "MQ" [arrowhead=normal,arrowtail=normal];`
` "MQ" -- "WAN" [arrowhead=normal,arrowtail=normal];`
` "MQ" -- "RLDRAM" [color=blue];`
` "MQ" -- "DDR3" [color=blue];`

{ rank=same; Fabric MQ WAN } } </graphviz>

PFE Capacity
------------

Each Trio PFE uses 72MB of RLDRAM for lookup memory, and 1GB of DDR3 for packet buffering memory. This is designed to allow the Trio PFE to handle up to 2 million routes.

[Category:Router Architecture](/Category:Router_Architecture "wikilink")