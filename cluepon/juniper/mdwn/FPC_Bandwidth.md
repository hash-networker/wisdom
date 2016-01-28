---
title: FPC Bandwidth
permalink: /FPC_Bandwidth/
---

The classic ([Martini architecture](/Martini_architecture "wikilink")) FPC has an advertised capacity of 3.2 Gigabits/sec. PIC configurations beyond 3.2 Gigabits/sec are supported, but are considered oversubscribed. The actual limit is dependent on the packet sizes due to [J-cellification](/J-cell "wikilink"). The advertised limit is a reasonable *realistic worst case* minimum which ignores a few pathological cases where performance might be somewhat lower. The [Gibson architecture](/Gibson_architecture "wikilink") FPCs have different limitations.

FPC Buffering
-------------

Buffer memory is stored on the FPC, and used as a distributed shared pool. Each FPC added to the system increases the amount of packet buffer memory available. The default SDRAM included on FPCs targets a maximum queue depth of 200 milliseconds.

Extreme conditions on Martini architecture
------------------------------------------

The worst case packet for a Juniper martini architecture FPC (the worst bandwidth to J-cell ratio) is 65 bytes, which requires 2 J-cells. At 2 J-cells per packet, 3.125M packets/sec will fill an FPC. A 65 byte packet weighs in at 103 bytes after full Ethernet overhead (8 bytes Preamble + Start of Frame Delimiter, 14 bytes Ethernet header, 65 bytes Payload, 4 bytes for Frame Check Sequence), which means that the line rate of 65 byte packets on a Gigabit Ethernet interface is (1000 Mbps / 824 bps/packet) = 1.21 Mpps. At this rate, it takes at least 2.575 GigE interfaces sending 65 byte packets at their line rate to fully exhaust an FPC.

Extreme conditions on Gibson architecture
-----------------------------------------

Gibson platforms become J-cell limited at the switch at just shy of 10gbit/sec with worst case packets. Because of the large per-packet overhead on 10gigabit ethernet links it is difficult to observe this limitation on a ethernet connected router. The effect is more measurable with OC-192.

This page needs much more expanded detail.

[Category:Router Architecture](/Category:Router_Architecture "wikilink") [Category:Stub](/Category:Stub "wikilink")