---
title: J-cell
permalink: /J-cell/
---

This page needs to be expanded

...

J-Cells and Bandwidth
---------------------

On [Martini architecture](/Martini_architecture "wikilink") routers, per-FPC bandwidth limitation comes from the [B-Chip](/B-Chip "wikilink") to [A1-Chip](/A-Chip "wikilink") link, which has a raw bandwidth of 4 Gigabits/sec in each direction. Each J-Cell has an overhead of 16 bytes per 64 bytes of payload, leaving only 3.2 Gigabits/sec for J-Cell payload. Thus each FPC can switch at most in each direction:

\(\left(\frac{\mbox{4 Gigabits/second}}{\frac{\left((\mbox{64 bytes} \times \frac{\mbox{8 bits}}{\mbox{1 byte}})+(\mbox{16 bytes} \times \frac{\mbox{8 bits}}{\mbox{1 byte}})\right)}{\mbox{J-cell}}}\right) = \mbox{6.25 Megacells/sec}\)

It is a common misconception that the limit comes from the SDRAM speed on the FPC, which is also 4 Gigabits/sec (64-bit data path at 125MHz). However, the packets sprayed across the FPCs SDRAM buffer memory are addresses by the [B-Chip](/B-Chip "wikilink") as 64-byte words, and do not have any J-Cell overhead.

See Also
--------

-   [FPC Bandwidth](/FPC_Bandwidth "wikilink")

[Category:Router Architecture](/Category:Router_Architecture "wikilink") [Category:Stub](/Category:Stub "wikilink")