---
title: Martini architecture
permalink: /Martini_architecture/
---

[thumb|400px|Logical view of the Martini architecture](/Image:Martini_arch.png "wikilink") The Martini architecture consists of several discrete [ASICs](/ASICs "wikilink"). The primary features include two [A-Chips](/A-Chip "wikilink"), one [B-Chip](/B-Chip "wikilink") per-FPC, and the Internet Processor ASIC ([Internet Processor II (CF-Chip)](/Internet_Processor_II "wikilink"), or [Internet Processor (C-Chip)](/Internet_Processor_(I) "wikilink") on older [M20](/M20 "wikilink") and [M40](/M40 "wikilink") Switch Boards), SRAM, DRAM, and a PowerPC processor. There are also a number of smaller control processors and control buses which our discussion will omit for clarity.

The Martini architecture is applied most directly in the [M40](/M40 "wikilink"), [M20](/M20 "wikilink"), [M5](/M5 "wikilink"), and [M10](/M10 "wikilink") routers, and is used in a scaled up form in the [M160](/M160 "wikilink") and a scaled down form in the [Calvin architecture](/Calvin_architecture "wikilink") routers, the [M7i](/M7i "wikilink") and [M10i](/M10i "wikilink").

Packet Flow
-----------

Physically [PICs](/PIC "wikilink") are installed into [FPCs](/FPC "wikilink"), which are seated in the chassis. Packets enter the router through a port on a PIC, and ASICs on the PIC perform the media-specific processing and decapsulation.

The packet is then handed to the Bi portion of the B chip of its FPC. The Bi removes the L2 packet headers and extracts the L3 headers. L2 header processing is performed using a set of microcode engines in the B-Chip. The packets are then segmented into 64 Byte [J-cells](/J-cell "wikilink"), and passed to the A1 chip.

A1 performs memory allocation for the shared memory across the FPCs, which is connected to the Bm portion of the B-chips on the FPCs. A1 hands the extracted notification, lookup key, and information needed to retrieve the packet to the C or CF-Chip (depending on the platform vintage). As a result, the cells of a packet are sprayed across all FPCs.

The CF-chip is equipped with a number of parallel microcode engines which perform a lookup on the key data against the CF connected SRAM. The CF then passes a notification and recovery information to the A2 chip. A2 hands the information to the Bo section of the correct FPC.

The egress Bo queues the notification for transmission. When the packet's time has come, Bo requests the needed cells from A1 which then collects the cells from the shared memory. Bo adds the correct L2 headers, then passes the packet to the outbound PIC.

The same Bm accessed SDRAM is also used for Bo notification queuing. Approximately a quarter of the SDRAM is reserved for this purpose.

[Category:Router Architecture](/Category:Router_Architecture "wikilink")