---
title: Engine 5
permalink: /Engine_5/
---

The Engine 5 linecard is actually Ciscos latest linecard despite the Engine 6 being released last year. This linecard is actually the SIP carrier card that Cisco is aiming to have their customers migrate to. The carrier card (SIP) is very similar to a VIP in the 7500 architecture. SPA modules are inserted into the SIP carrier card. It is also the only other linecard than the Engine 3 that is supported in IOS-XR.

-   10Gbps to the fabric

Engine 5 SIP - lists as **L3 Engine: 5 - ISE OC192 (10 Gbps)** or **L3 Engine: 5 - ISE 10 Gbps**

| Card        | Part Name (FRU) | Part Number (MAIN) | Codename |
|-------------|-----------------|--------------------|----------|
| SIP carrier | 12000-SIP-600   | 800-26102-02       | Bluenose |
| SIP carrier | 12000-SIP-601   | 68-2647-02         | ?        |

Engine 5 SPA:

| Card                              | Part Name (FRU)   | Part Number (MAIN) | Codename  |
|-----------------------------------|-------------------|--------------------|-----------|
| 1 Port 10-Gigabit Ethernet SPA    | SPA-1XTENGE-XFP   | Unknown            | Scorpion  |
| 5 Port Gigabit Ethernet SPA       | SPA-5X1GE         | 68-2236-02         | Cricket   |
| 10 Port Gigabit Ethernet SPA      | SPA-10X1GE        | 68-2453-02         | Damselfly |
| 1 Port OC192 POS SPA (XFP optics) | SPA-OC192POS-XFP  | 68-2191-04         | Iguana    |
| 1 Port OC192 POS SPA (LR optics)  | Unknown           | Unknown            | Chameleon |
| 4-port T3/E3 Serial SPA           | SPA-4XT3/E3       | 68-2170-02         | ?         |
| 2-port OC48/STM16 POS/RPR SPA     | SPA-2XOC48POS/RPR | 68-2226-01         | ?         |

[Category:GSR Linecards](/Category:GSR_Linecards "wikilink")