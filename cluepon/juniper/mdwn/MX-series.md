---
title: MX-series
permalink: /MX-series/
---

[thumb|right|Juniper MX-series Routers](/Image:MX_family.jpg "wikilink")

The MX-Series are a family of Carrier Ethernet Switch Routers (CESRs), originally launched in early 2007 with the MX960, and followed later with smaller MX480 and MX240 systems.

Second-gen MX-series (MX 3D) arrived late 2009 and included new chipset ("Trio") with rich IP/MPLS processing and subscriber management capabilities, 120G/slot linecards and a new compact chassis (MX80).

All of MX-Series chassis share common DPC and SCB components for interfaces and forwarding. MX-series sheet metal and some cards are also reused in [SRX](/SRX-series "wikilink") series high-end security applicances.

[thumb|right|Juniper MX-series Chart](/Image:MX_Series_resume.jpg "wikilink")

MX-series Products

-   [MX960](/MX960 "wikilink")
-   [MX480](/MX480 "wikilink")
-   [MX240](/MX240 "wikilink")
-   [MX80](/MX80 "wikilink")

First Generation
----------------

The MX960 became Juniper's first ethernet-centric "services switch" product, and marked the introduction of the [DPC](/DPC "wikilink") interface card. Interface cards are built around Juniper's proprietary full-duplex 10gbps NPU [I-Chip](/I-Chip "wikilink"), which integrates the functionality of the [Internet Processor II](/Internet_Processor_II "wikilink") and several other chips on one die (including [Tunnel PIC](/Tunnel_PIC "wikilink") functionality). Individual 10GE interfaces can be configured to provide tunnel services (the physical interface is not available for use). On the 40xGigE DPC, a 1gbps "11th PIC" on each of the four I-Chips can be used for this purpose without impacting the 10 physical interfaces controlled by the chip. Initially, I-chip was developed for edge functionality similar to Juniper [M-series](/M-series "wikilink"), so Juniper added EZChip NP-2 NPU for Ethernet-specific L2 functionality and (optional) rich queuing to MX linecards. This tandem of two highly integrated chips ensured speed and feature growth potential, with density sufficient to provide full-duplex 40G forwarding on one DPC card with four I-chip/EZ complexes. This design quickly gained market from service providers, who were pleased with rich IP/MPLS feature set and clear Ethernet focus of the device.

Second Generation (MX3D)
------------------------

The second generation of MX hardware was introduced by Juniper in 4Q2009 and is built around a brand-new "Trio" chipset. There are no 3rd party NPUs in the design. The new 30Gbps (full-duplex) NPU is the company's first chip oriented towards the "common edge" and integrating rich L2/L3 and broadband subscriber management functionality. The new network cards (MPC) employ a constellation of four "Trio" chips for full-duplex throughput of 120 Gbps per slot (with support for up to 16x10GE ports with 4:3 oversubscription ratio), though under some conditions users may be able to achieve better [oversubscription](/Trio_oversubscription "wikilink").

Competition
-----------

At launch, MX-series mainly competed with Cisco 7600 and Alcatel 7750. The second generation of MX-series (MX3D) is now positioned against A7750 equipped with IOM3 hardware (50Gbps full-duplex per slot), and the Cisco ASR9000-series platform.

[Category:Hardware](/Category:Hardware "wikilink") [Category:MX-series](/Category:MX-series "wikilink") [Category:Stub](/Category:Stub "wikilink")