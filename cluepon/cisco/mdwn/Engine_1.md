---
title: Engine 1
permalink: /Engine_1/
---

The Engine 1 (Salsa) linecard is similar to the [Engine 0](/Engine_0 "wikilink") linecard in that it is also software based (all packets handled by CPU processing). It is believed that this linecard was made as a "Time-To-Market" product as some linecards developed as Engine 1 are no longer being sold today. Specifically, Sprint was known to operate a 4 Port OC-12 linecard where the Engine 1 had the capability to connect to the fabric at OC48 (2.5Gbps speeds).

The 8 Port Fast Ethernet linecard is the only publically available Fast Ethernet linecard for the GSR today. There were plans for the Engine 4+ "Tango" line card to have EPA modules to support Fast Ethernet, but it appears have fallen through and this line card is the only option for copper Fast Ethernet. The one-port Gigabit Ethernet linecard was also plagued with problems in production environments due to earlier code not supporting common features (ie. ACLs). In addition, some features such as MAC accounting on the 1 Port Gigabit Ethernet card would render the line card useless at 600-700Mbps.

Contrary to the name, GE-SX/LH-SC takes a GBIC.

-   622Mbps fabric connection
-   2.5Gbps fabric connection on some line cards

Engine 1 - lists as **Engine: 1 - Standard OC48 (2.5 Gbps)**

| Card                              | Part Name (FRU)    | Part Number (MAIN) | Codename |
|-----------------------------------|--------------------|--------------------|----------|
| 8 Port Fast Ethernet Copper       | 8FE-TX-RJ45-B      | 800-7801-01        | Eiffel   |
| 1 Port Gigabit Ethernet (non ECC) | GE-GBIC-SC-A       | Unknown            | Unknown  |
| 1 Port Gigabit Ethernet (non ECC) | GE-SX/LH-SC        | Unknown            | Unknown  |
| 1 Port Gigabit Ethernet (ECC)     | GE-GBIC-SC-B       | 800-3955-03        | Spigel   |
| 2 Port OC12 DPT (Single-Ring DPT) | OC12/SRP-MM-SC-B   | 800-07796-01       | Unknown  |
| 1 Port OC48 TTM                   | OC48/POS-SR-SC (?) | 800-4701-02        | Unknown  |
| 4 Port OC12 TTM                   | Unknown            | Unknown            | Unknown  |

[Category:GSR Linecards](/Category:GSR_Linecards "wikilink")