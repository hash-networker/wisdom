---
title: Unofficial hardware upgrades
permalink: /Unofficial_hardware_upgrades/
---

M20 SSB
-------

A 64MB IP2 SSB on an M20 can be upgraded to 128MB by adding a second 64MB DIMM. You can grab one from an unused SSB or buy one pretty cheaply. The specifications are:

-   64MB (8Mx72)
-   3.3v 168 pin
-   EDO
-   Unbuffered
-   ECC
-   60ns, 4K refresh
-   9 chips

DIMMs taller than 1.25 inches probably won't fit.

[Source for cheap compatible DIMMs](http://www.oempcworld.com/Merchant2/merchant.mvc?Screen=PROD&Product_Code=64M-EDO-DIMM-ECC) Tested and works.

M5/M10 FEB
----------

[thumb|right|Factory original SDRAM on M5/M10 FEB](/Image:M5-M10-FEB-SDRAM.jpg "wikilink") [thumb|right|DIMMs that are too tall won't work](/Image:M5-M10-FEB-SDRAM-BAD.jpg "wikilink")

A M5 or M10 FEB can be upgraded from 64MB to 128MB of SDRAM. There is only one DIMM socket on the board, so you must replace the DIMM entirely. The specifications are:

-   128MB 144-pin SODIMM
-   PC100 CL2
-   ECC
-   Must be 9 chips
-   Must be very low profile

The memory module is electrically compatible with a Cisco compatible part MEM-NPE-400-128MB or MEM-MSFC2-128MB. However, the MEM-NPE-400-128MB is physically too large to fit. A third party version of the MEM-MSFC2-128MB works fine and fits correctly.

[Source for cheap MEM-MSFC2-128MB compatible modules](http://www.memorydealers.com/12cisapcat60.html) Tested and seem to work in M5/M10

It is possible to use larger modules than 128MB, for example 512MB. Tested to work is a module from SMART, partno SM572648578D9BPID0 with 512MB. However only 128MB will be recognized by JunOS.

M7i/M10i cFEB
-------------

A M7i/M10i cFEB can be upgraded from 128MB to 256MB of SDRAM. There is only one SO-DIMM socket on the board, so you must replace the DIMM entirely. The specifications are:

-   256MB 144-pin SODIMM
-   PC133
-   ECC
-   Must be very low profile

The memory module is compatible with a Cisco compatible part MEM-MSFC2-256MB. A third party version of the MEM-MSFC2-256MB works fine and fits correctly.

Juniper uses memory from SMART (Partno SM5JPN32M72001) in the original MEM-FEB-256-S upgrade kit for M7i/M10i.

Memory Testing
--------------

### !!! BEWARE !!!

**1. The SO-DIMM module shown above is the CPU SDRAM of the PPC. The diag below tests the soldered SDRAM near the B-CHIP and SRAM.**

**2. The FPC and PICs _MUST_ be OFFLINE or UNCONFIGURED in the Juniper (CLI), or you WILL get random failures in this test that do NOT indicate a problem.**

You can test FPC DRAM (not SSB DRAM) from the SSB's diagnostic interface. For example, using the following commands, you can test FPC 0's DRAM:

`* start shell user root`
`* vty ssb/feb`
`* bringup chassis slot-state 0 diag`
`* diagnostic set mode manufacturing`
`* diag clear log`
`* diag bchip 0 sdram `

`SBR(router vty)# diag bchip 0 sdram`
`[Waiting for completion, a:abort, p:pause]`
`B SDRAM (Slot 0) test`
`phase 1, B SDRAM (Slot 0) test: Address Test`
`phase 2, B SDRAM (Slot 0) test: Pattern Test`
`phase 3, B SDRAM (Slot 0) test: Walking 0 Test`
`phase 4, B SDRAM (Slot 0) test: Walking 1 Test`
`phase 5, B SDRAM (Slot 0) test: Mem Clear Test`
`B SDRAM (Slot 0) test completed, 1 pass,  0 errors`

Afterwards disable diag mode:

`* diagnostic set mode bring-up`
`* bringup chassis slot-state 0 on-line`