---
title: Engine 3
permalink: /Engine_3/
---

The Engine 3 (or ISE, Internet Services Engine) was Ciscos attempt to make a feature-rich linecard targeted for the edge of the network where multiple features are required. It was designed with a programmable hardware-based forwarding engine. TCAMs are utilized on this platform to deliver advanced feature processing. The ALPHA (Advanced Layer 3 Packet Handling ASIC) is what handles the high-speed feature processing with its own TCAM each. The only drawback is that the linecard still uses a 2.5Gbps connection to the fabric. This makes such things as the 4 Port Gigabit Ethernet a sad encore performance of the 3 Port Gigabit Ethernet linecard.

This linecard is one of two GSR linecard types that is supported in IOS-XR for the GSR platform. Rumor is because of its architecture utilizing TCAMs is the reason why the Engine 3 is supported.

The Engine 3 linecards also had a late introduction to the market. For various reasons, the [Engine 4](/Engine_4 "wikilink") linecard was released first before most of the Engine 3 models were ready.

There is also a carrier card (SIP) that operates as an Engine 3 linecard. This carrier card can have SPAs be inserted to offer a similar architecture of PAs and VIPs.

-   2.5Gbps to the fabric

Engine 3:

-   4 Port OC12 POS - Codename: Jag48
-   4 Port Channelized OC12 -&gt; OC3 -&gt; DS3 - Codename: Jag48
-   8/16 Port OC3 POS - Codename: Jag48
-   1 Port OC48 POS - Codename: Jag48
-   1 Port Channelized OC48 -&gt; DS3 - Codename: Jag48
-   4/8 Port OC3 POS - Codename: Silverado
-   1 Port Channelized OC12 -&gt; DS1 - Codename: Frosbite
-   4 Port Gigabit Ethernet - Codename: Tetra
-   4 Port OC3 ATM - Codename: Arava
-   4 Port OC12 ATM - Codename: Pinnacle
-   4 Port OC12 DPT - Codename: Merlin Edge

Engine 3 SPAs:

-   2 Port Channelized DS3 SPA - Codename: Patriot
-   4 Port Channelized DS3 SPA - Codename: Patriot
-   2 Port T3/E3 SPA - Codename: Javelin
-   4 Port T3/E3 SPA - Codename: Javeline

[Category:GSR Linecards](/Category:GSR_Linecards "wikilink")