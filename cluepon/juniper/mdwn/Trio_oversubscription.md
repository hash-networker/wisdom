---
title: Trio oversubscription
permalink: /Trio_oversubscription/
---

[Trio](/Trio_architecture "wikilink") hardware has been marketed as a "30G PFE", but under certain conditions it may be able to achieve significantly higher (or lower) thruput. Some networks find that they are able to support reasonably close to 40Gbps, allowing them to use all four 10GE ports per Trio PFE.

Fabric Capacity
---------------

The Juniper [MX-series](/MX-series "wikilink") platform uses a model of fully distributed [PFEs](/PFE "wikilink") on each line-card, interconnected to each other via SCB fabric cards. The SCB cards contains two SF (Stoli Fabric) ASICs, which each function as a fabric "plane" to interconnect line-cards. Each SF ASIC supports a total of 128 "HSL2" (2.5Gbps) links, giving each SF chip a total capacity of 320 Gbps. These HSL2 links can be configured to run in either HSL2x2 mode, in order to provide (64) 5Gbps channels, or in HSL2x4 mode, in order to provide (32) 10Gbps channels.

On an MX960 (which can support up to 12 slots), and when using cards with 4 PFEs each (such as classic DPC cards, and the 16-port 10GE Trio card), the HSL2x2 mode is used to support up to 48 PFEs per chassis. Because each SF ASIC provides 5Gbps per PFE, and there are two SF ASICs per SCB, each SCB provides 10Gbps of fabric capacity per PFE. On MX240 and MX480 chassis, or when using cards with less than 4 PFEs each (such as the MPC1 and MPC2 Trio cards), the HSL2x4 mode is used, allowing each SCB to provide 20Gbps of fabric capacity per PFE.

MX240 and MX480 chassis support 2 SCB cards, giving a total fabric capacity of 40Gbps per PFE for all card types. The MX960 chassis supports 3 SCB cards, giving a total fabric capacity of 30Gbps per PFE for 4-PFE-per-card cards, or 60Gbps per PFE for smaller cards. However, the older I-chip cards are only capable of talking to 2 SCB cards simultaneously, with the 3rd SCB card originally intended as an N+1 spare. Trio cards can talk to each other over all 3 SCBs, but users who mix I-Chip and Trio cards in the same chassis may experience limitations which prevent cards from achieving line-rate. For example, on an MX960 with 16-port 10GE Trio cards, each 4-port 10GE PFE will only be able to achieve 20Gbps when talking to I-chip cards, significantly less than the officially rated 30Gbps number.

Trio cards are also capable of doing local switching, so traffic which goes from one interface to another on the same Trio PFE will not consume fabric resources.

MQ Capacity
-----------

Each MQ-chip is capable of handling approximately 70Gbps of total unidirectional traffic, though the exact performance depends on the packet size. This bandwidth is shared between the fabric and WAN interfaces, so with 1:1 bidirectional traffic each MQ-chip is capable of handling 35Gbps. If the traffic is not bidirectional, the MQ-chip is capable of achieving line-rate 40Gbps per PFE (e.g. 40Gbps in, 30Gbps out).

LU Capacity
-----------

Each LU-chip is capable of processing approximately 55Mpps, though the exact performance depends on the features enabled. Four 10GE interfaces doing line-rate traffic would consume 4 \* 14.88Mpps = 59.52Mpps, so even under the optimal conditions the LU-chip is not capable of handling 4 ports at line rate.

[Category:Router Architecture](/Category:Router_Architecture "wikilink")