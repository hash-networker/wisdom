---
title: Fabric
permalink: /Fabric/
---

**Fabric on the GSR:**

The GSR utilizes a crossbar switch fabric. This provides connectivity between line cards and to the route processors.

64 Byte Cells are used within the switching fabric. The "Cisco Cell" is comprised of an 8 byte header, 48 byte payload and 8 byte CRC. It takes roughly 160 nanoseconds to transmit a cell.

Unicast and Multicast data & routing protocol packets are transmitting over the fabric.

Multicast packets are replicated within the fabric and transmitted to the destination line cards by means of partial fufillment. (Busy line cards are sent copies later when they are not busy).

Local traffic on a line card still has to transit the fabric.

2.5Gbps fabric has a 16 x 16 crossbar and uses the ESLIP algorithm for scheduling.

10Gbps fabric has a 64 x 64 crossbar and uses multichannel matching algorithm for scheduling.

**Switch Fabric Cards (SFC) and Clock Scheduler Cards (CSC):**

Typically 5 switching fabrics (3 SFC and 2 CSC).

For some models (12410), there is a combined SFC/CSC card.

For proper operation, 4 fabric cards must be active at a time. This includes 3 CSC.

-   SCA - Scheduler Control ASIC (only on CSC). Arbitration of line card and RP request to transmit data through fabric.

**Cisco Cell In-Depth**

Cisco Cell Header - 4 bytes

Payload Header - 4 bytes

Payload - 48 bytes

CRC - 8 bytes

FIA (Fabric Interface ASICs) performs segmentation and reassmbly for Cisco cells.

The cell header carries information regarding the disposition of the cell such as if cell starts or ends a packet.

Sending a Cisco Cell:

During each clock period (160ns)

-   Sending line cards send a *fabric request* to the SCA
-   SCA runs the *ESLIP* scheduling algorithm
-   SCA returns a *fabric grant* to the line card
-   Line card responds with a *fabric grant accept*
-   SCA sets the crossbar for that cell clock
-   SCA listens for *fabric backpressure* to stop scheduling for a particular line card
