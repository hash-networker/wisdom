---
title: Route Processors
permalink: /Route_Processors/
---

**Route Processors**

The route processor control data path for line cards via instructions transmitted over IPC on the fabric or the MBUS. The fabric method is the default and preferred method.

The MBUS connection is mainly present for the RP to download a boostrap image, collect or load diagnostic information or perform general maintenance.

The RP runs routing protocols, builds and distributes the routing table to line cards, handles general maintenance functions (booting line cards) and handles L2 control messages (PPP, HDLC, Frame-Relay, ARP, ATM OAM).

The RP maintains its own copy of the RIB, known as *RP-FIB*.

Routing updates are forwarded from the RP-FIB to the line cards *LC-FIB*.

Each LC-FIB entry corresponds to an interface which contains a MAC encapsulation string, output interface and MTU.

FIB distribution is accomplished via reliable IPC (XDR) updates.

Updates are unicast across the fabric to all line cards.

**Hardware**

**GRP**

-   R5000 CPU - 200Mhz(aka P4)
-   64 to 256(512)Mb of EDO RAM
-   MBUS Module
-   Tiger ASIC
-   CSAR ASIC
-   FIA ASIC
-   SLI ASIC
-   Power Modules
-   Two variants, GRP and GRP-B. GRP-B permits memory expansion to 512MB. It is reported that a standard GRP can be upgraded to 512MB as long as the bootrom image is upgraded to a specific version.

**PRP-1**

-   MPC7450 PowerPC - 133Mhz IO, 667 Mhz Core
-   32KB L1 cache, 256 KB L2 cache
-   2MB external L3 cache using PB2 SRAMs @ 133 Mhz
-   512MB to 2GB of PC133 SDRAM
-   2 PCMCIA Type I or Type II slots
-   64MB, 128MB or 1GB flash disks - 20MB linear flash still supported
-   64MB bootflash
-   2M NVRAM
-   4 FrFab queues, 1 ToFab queue

**PRP-2**

-   1.3Ghz PowerPC CPU
-   30-100% performance improvement over PRP-1
-   Default memory is 1GB
-   At 12.0(30)S, 4GB of memory will be supported
-   Gigabit Ethernet management port supported in 12.0(30)S and beyond
-   2 BITS input ports (30S and beyond)
-   40GB Disk Drive optional
-   1GB flash on-board (for IOX)
-   8 FrFab queues, 2 ToFab queues
