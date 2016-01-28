---
title: Calvin architecture
permalink: /Calvin_architecture/
---

[thumb|right|Juniper ABC-Chip ASIC](/Image:ABC-Chip.jpg "wikilink")

The **Calvin architecture** is a minimal respin of the [Martini architecture](/Martini_architecture "wikilink") created for the [M7i](/M7i "wikilink") ([Codename:](/Codenames "wikilink") *Calvin*) and [M10i](/M10i "wikilink") ([Codename:](/Codenames "wikilink") *Hobbes*). Its functionality is almost identical to [Martini](/Martini_architecture "wikilink"), the changes were primarily made to reduce costs.

The [Martini architecture](/Martini_architecture "wikilink") consists of several discrete [ASICs](/ASICs "wikilink"): Two [A-Chips](/A-Chip "wikilink"), one [B-Chip](/B-Chip "wikilink") per FPC, and the [CF-Chip](/Internet_Processor_II "wikilink"). In Calvin these were combined into the *ABC-Chip* for a large cost reduction and lower power requirements.

Other changes include:

-   Improved silicon process.
-   Reduced board space.
-   Support for lower cost and denser external memory. including:
    -   DDR SDRAM
    -   ZBT SRAM
-   Only two banks of SRAM, rather than four.
    -   This reduces lookup bandwidth, so the number of lookup processors in the CF were reduced as well.
    -   As a result, ABC-Chip is only able to forward around 16Mpps vs [CF-Chip's](/Internet_Processor_II "wikilink") 40Mpps.
-   Only two banks of DDR SDRAM rather than four banks of EDO
    -   Because of DDR, it provides the same bandwidth.
-   [B-Chip](/B-Chip "wikilink") component in ABC has double the microcode space from B-3.0.

The forwarding engine board, the [CFEB](/CFEB "wikilink"), in Calvin architecture routers contains the entire PFE minus the PICs themselves. The CFEB used in the [M7i](/M7i "wikilink") and the [M10i](/M10i "wikilink") is the same part. Since the M7i only has four pic slots the second B-Chip is used to support the [fixed PIC](/FIC "wikilink"), on-board [tunnel services](/Tunnel_PIC "wikilink"), and the optional on board [Adaptive Services module](/Adaptive_Services_module "wikilink").

[Category:Router Architecture](/Category:Router_Architecture "wikilink") [Category:Image needed](/Category:Image_needed "wikilink") [Category:Hardware](/Category:Hardware "wikilink") [Category:ASICs](/Category:ASICs "wikilink")