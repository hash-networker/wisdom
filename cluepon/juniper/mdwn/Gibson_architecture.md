---
title: Gibson architecture
permalink: /Gibson_architecture/
---

The **Gibson architecture** was initially designed for the [T640](/T640 "wikilink"), but has since been used in the [T320](/T320 "wikilink") and [M320](/M320 "wikilink") as well. It represents a departure from the flat shared memory model of the [Martini architecture](/Martini_architecture "wikilink") in that it utilizes multiple [packet forwarding engines](/PFE "wikilink") with their own buffering, interconnected by a crossbar ([Clos](/Clos "wikilink") in the case of multichassis) switching fabric. The Gibson architecture was designed to support improved port density and packet forwarding performance, and to support [multichassis](/TX_Matrix "wikilink") operation for very high scalability.

Packet flow overview
--------------------

Packets enter a Gibson router via [PICs](/PIC "wikilink") ([type 1](/type_1 "wikilink"), [type 2](/type_2 "wikilink"), or [type 3](/type_3 "wikilink") PICs are supported depending on [FPC](/FPC "wikilink") type); on the FPC the PFE segments the packets into [J-cells](/J-cell "wikilink"), extracts a lookup key which it sends to the IP ASIC, and stores the cells in memory. The IP ASIC makes a routing decision and directs the cells to their egress PFE, where they are again stored in memory. A second route lookup is performed, and the cells are reassembled and directed to an outbound interface.

For example, a packet flow from interface 0/0/0 to 1/1/0:

<graphviz> graph gibsonoverall { fontsize=10

` "PIC 0/0" -- "PFE 0" [color=red,arrowhead=normal];`
` "PIC 0/1" -- "PFE 0";`
` "PIC 1/0" -- "PFE 1";`
` "PIC 1/1" -- "PFE 1" [color=red,arrowtail=normal];`
` "PFE 0" -- Switch [color=red,arrowhead=normal];`
` "PFE 1" -- Switch [color=red,arrowtail=normal];`

} </graphviz>

Routing Engine (RE)
-------------------

The Gibson architecture uses a routing engine just like the [Martini architecture](/Martini_architecture "wikilink").

Packet Forwarding Engine (PFE)
------------------------------

Each FPC contains one ([M320](/M320 "wikilink"), [T320](/T320 "wikilink")) or two ([T640](/T640 "wikilink")) packet forwarding engines. The PFE, which consists of several custom ASICs and high speed memory, is the heart of the packet forwarding process.

The Gibson architecture uses several ASICs:

-   L2/L3 Packet Processing ASIC
-   Switch Interface ASIC (SI)
-   Internet Processor ASIC (IP)
-   Queuing and Memory ASIC (Q&M)
-   Fabric ASIC

Several of these ASICs can operate in multiple modes, which enabled Juniper to use the same part in multiple places in their design.

### Detailed PFE packet flow

An inbound packet first receives media specific handling on its ingress PIC. It is then passed to the L2/L3 ASIC which performs:

-   Control information extraction (protocol type)
-   Input accounting
-   L2 header removal
-   Segmentation into J-Cells (Jcell flows shown in blue below)

The L2/L3 ASIC passes the cells to a Switch interface ASIC operating in network mode. While in network mode, the SI ASIC performs:

-   Routing lookup key extraction; the SI ASIC can be programmed to extract any needed information.
-   Sprays the cells to the Q&M memory ASICs and collects the locations of the stored cells.
-   Combines the locations, the lookup key, and interface information into a [notification](/notification "wikilink") (shown in red below).
-   Sends notification to the IP ASIC.

The IP ASIC used in the Gibson is a new design based on the [IP2](/IP2 "wikilink"). It is able to perform 80 million packet forwarding operations per second. The ASIC uses the lookup key in the notification to determine the correct handling of the incoming packet. It then amends the notification with its routing decision, priority information, rewrite actions, etc, and passes the notification on to the Q&M ASIC in queue mode.

While in Queue mode, the Q&M ASIC:

-   Queues notifications in 4 banks of DRAM (not shown in chart)
-   Performs priority queuing
-   Performs RED
-   Polices and rate-shapes traffic
-   Forwards a notification to the fabric mode of the switch interface ASIC when it is time to dequeue the packet

After receiving a notification, the switch interface ASIC in fabric mode performs:

-   Fetches packet data from Q&M memory using the addresses in the notification
-   Adds a header and CRC to each sell. The header contains the source and destination PFE, and a sequence number used to prevent cell reordering
-   Sends and receives flow control across the fabric
-   Transmits cells towards the fabric

The egress PFE performs the reverse of the same operations.

<graphviz> digraph gibsonPFE {

`    size="9.5,5.5"`
`    ratio=".75";`
`    node [fontsize=4];`

`   subgraph clusterA {`
`      "Type-3 PIC 0" [URL="type-3"];`
`      "Type-3 PIC 1" [URL="type-3"];`
`    }`

`    subgraph clusterB {`
`      color=blue;`

`      "T-Series IP2" [group=core,URL="IP2"];`
`      "SI (fabric)" [group=core];`
`      "Q&M\n\n(Queue mode)" [group=core];`
`      "SI (net)" [group=core];`

`      "T-Series IP2" -> "SRAM" [arrowtail=normal,weight=5];`
`       `
`     `
`      "L2/L3 ASIC";`
`      "L2/L3 ASIC";`

`      "Type-3 PIC 0" -> "L2/L3 ASIC" [arrowtail=normal,color=green];`
`      "Type-3 PIC 1" -> "L2/L3 ASIC" [arrowtail=normal,color=green];`
`      "L2/L3 ASIC" -> "SI (net)" [arrowtail=normal,color=blue];`

`      "SI (net)" -> "T-Series IP2" [color=red,weigh=4];`

`      "T-Series IP2" -> "Q&M\n\n(Queue mode)" [color=red,weigh=4];`
`      "Q&M\n\n(Queue mode)" -> "SI (fabric)" [color=red,weight=5];`
`      "Q&M\n\n(Queue mode)" -> "SI (net)" [color=red,weight=5];`
`      "SI (fabric)" -> "T-Series IP2" [color=red,weight=4];`
`  `
`      "SI (fabric)" -> "Q&M(MM)0" [arrowtail=normal,color=blue];`
`      "SI (fabric)" -> "Q&M(MM)1" [arrowtail=normal,color=blue];`
`      "SI (fabric)" -> "Q&M(MM)2" [arrowtail=normal,color=blue];`
`      "SI (fabric)" -> "Q&M(MM)3" [arrowtail=normal,color=blue];`
`      "SI (net)" -> "Q&M(MM)0" [arrowtail=normal,color=blue];`
`      "SI (net)" -> "Q&M(MM)1" [arrowtail=normal,color=blue];`
`      "SI (net)" -> "Q&M(MM)2" [arrowtail=normal,color=blue];`
`      "SI (net)" -> "Q&M(MM)3" [arrowtail=normal,color=blue];`

`      "Q&M(MM)0" -> "Bank 0" [arrowtail=normal,weight=5];`
`      "Q&M(MM)1" -> "Bank 1" [arrowtail=normal,weight=5];`
`      "Q&M(MM)2" -> "Bank 2" [arrowtail=normal,weight=5];`
`      "Q&M(MM)3" -> "Bank 3" [arrowtail=normal,weight=5];`
`      `
`    }`
`    subgraph clusterC {`
`      "Fabric\n\nASIC";`
`      `
`      "SI (fabric)" -> "Fabric\n\nASIC" [arrowtail=normal,color=blue]; `
`    }`
`    label="Gibson PFE";`

} </graphviz>

Switch Fabric
-------------

A switch fabric interconnects all the PFEs. The switch fabric consists of 4+1 'slices'. Cells are sprayed across the slices. With 4 slices operational a T640's fabric is nonblocking. The system can run with fewer slices operating but with degraded performance.

### Flow control

-   Requests
-   Grants

[Category:Router Architecture](/Category:Router_Architecture "wikilink")