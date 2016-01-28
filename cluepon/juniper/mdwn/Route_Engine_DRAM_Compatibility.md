---
title: Route Engine DRAM Compatibility
permalink: /Route_Engine_DRAM_Compatibility/
---

RE-1.0 Generic DRAM
-------------------

-   2x128MB 72-pin EDO SIMMs

The RE-1.0 has been confirmed (In Production) to have a max memory of 512MB with the addition of 2x128 for a total of 512MB. The RE comes with 2x128MB but has four slots allowing for the additional two 128MB SIMMs. Call any Memory house, they will have it in stock (Old Stock). 128MB 72pin SIMM, I can't verify if it is ECC but if I remember correctly it is not required.

RE-2.0 Generic DRAM
-------------------

-   256MB PC100 168-pin ECC non-buffered non-registered low-density low-profile DIMMs

The [RE-2.0](/RE-2.0 "wikilink") uses the 440BX chipset, which has a maximum supported memory limit of 768MB, and comes with 3 DIMM slots. As shipped from Juniper, the RE-2.0 only comes with 256MB DIMMs (one DIMM for the 256MB model originally sold with M5/M10, three DIMMs for the 768MB model). A 512MB DIMM can be used, with the caveat that if the total memory goes over 768MB an entire DIMM is ignored to bring it under the limit (for example, 2x512MB DIMMs = 512MB detected, 1x512MB DIMM + 1x256MB DIMM = 768MB detected).

Crucial CT32M72S4D7E memory has been confirmed to work with the RE-2.0. Both PC100 and PC133 memory are supported (though the memory runs as PC100), and most 256MB DIMMs are supported. The RE will run without ECC memory, but it is highly advisable to use ECC memory in production environments.

Kingston Technologies KVR100X72C2L/256 memory has also been confirmed to work (ECC, PC100, low-profile, non-registered). The registered version of the same DIMM was recognized by the RE, but only 1 DIMM was seen when 2 were inserted.

RE-3.0 Generic DRAM
-------------------

RE-3.0 uses PC133 ECC SDRAM (not/un- registered). Kingston KVR133X72C3L/512 (x 4 pieces =2gb) is confirmed to do the job. In general you need to make sure to use low profile memory.

RE-4.0 Generic DRAM
-------------------

The RE-4.0 uses most likely DDR1 PC3200 registered ECC memory (according to the datasheet of the installed genuine SMART memory). Kingston KVR400X72RC3A/512 should work.

RE-5.0 Generic DRAM
-------------------

-   256M PC100 32Mx72 168-pin ECC non-buffered non-registered CL3 DIMMs

One source of OEM qualified parts is UNIGEN (Fremont, CA) p/n UG532S7448JJ-PLKA. This is a part that was specifically qualified by Kontron (the manufacturer of the RE-5.0) for use with their products.

Some DIMMs do not seem to work correctly, only detecting half the memory. An example of this is Kingston KVR133X72C3L/256.

Dane-Elec DP100-072321E PC100 256MB ECC is tested to work.

Crucial CT32M72S4D7E PC133 256MB ECC is tested to work.

Micron MT18LSDT32272AG-133G3 PC133 256MB ECC is tested to work.

RE-5.0+ Generic DRAM
--------------------

-   RE-5.0+ comes configured with the maximum supported amount of memory.

RE-J2350-2500 Generic DRAM
--------------------------

-   Crucial Part Number CT6464Z40B is tested to work in J2320/J2350 routers.
-   Kingston Part Number 3424603 (KVR400X64C3AK2/2G) 2x1GB Kit tested & works in J2320 (tested 2009-07-22, JUNOS 9.5R2.7).

Specs: DDR PC3200, CL=3, Unbuffered, NON-ECC, DDR400

RE-J2000 Generic DRAM
---------------------

-   256MB NON-ECC PC2100 DDR-RAM is tested to work on the J2300.

RE-J4350 Generic DRAM
---------------------

-   Crucial Part Number CT6464Z40B works on a J4350 (2009-09-08).
-   Kingston P/N KVR400X64C3A/512 (DIMM 184-pins, PC3200, DDR400, CL3, NON-ECC, unbuffered, 2.5V) works on a J4350 (2010-06-09).

RE-J6350 Generic DRAM
---------------------

-   Micron P/N MT8VDDT6464AY-40BF4 (DIMM 184 PC3200 DDR400, CL3, NON-ECC, unbuffered, 2.5v) tested 2010-08-17

[Category:Stub](/Category:Stub "wikilink")