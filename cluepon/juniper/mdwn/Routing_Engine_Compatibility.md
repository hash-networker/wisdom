---
title: Routing Engine Compatibility
permalink: /Routing_Engine_Compatibility/
---

RE-2.0 and RE-3.0 Compat Issues
-------------------------------

There is one HUGE issue with using RE-2.0 and RE-3.0 RE's in the same chassis; otherwise it works just fine.

If you're running RE-3.0 as master, and insert RE-2.0 as a slave in an empty slot, the master RE crashes immediately. There is no workaround. Juniper is aware of this problem (PR 27851), with apparently no intention to fix the crashing.

Other experiences?
------------------

The routing engines use the following harddiscs:
Original Routing Engine (233)--[ST310212A](http://www.seagate.com/support/disc/ata/st310212a.html), [ST36421A](http://www.seagate.com/support/disc/ata/st36421a.html), and Quantum Fireball SE6.4A (Enhanced ATA/IDE; 6.4 GB)
Routing Engine 333--IBM-DARA-212000, IBM-DCXA-210000, IBM-DJSA-210, IBM-DJSA-220, and Toshiba MK2016GA
Routing Engine 600--Fujitsu MHN2300AT, Fujitsu MHL2300AT, Fujitsu MHM2100AT, and Fujitsu MHR2030AT
(taken from release notes of junos 5.5)
They can easily be changed.

CF can be easily changed on [RE-600](/RE-600 "wikilink"), it's possible on [RE-333](/RE-333 "wikilink") too but it sits
below the hdd thus you have to completly remove all parts of the RE to gain access to it. Its not soldered, its just plugged in.
The flash on the [RE-233](/RE-233 "wikilink") is not very easy to replace/modify/remove because it is mounted to be difficult to remove. It is a circuit board plugged in via a ribbon cable and not a standard pluggable CF or PCMCIA flash device.