---
title: CFEB
permalink: /CFEB/
---

[thumb|right|Compact Forwarding Engine Board (C-FEB)](/Image:C-FEB_Front.jpg "wikilink")

Capacity
--------

    > How many routes should cfeb support ?

    The standard CFEB can hold 450,000 forwarding entries in the
    8MB SSRAM it has.

    The Enhanced CFEB has 2x 64MB RLDRAM's, which can hold
    substantially more entries. I'm not exactly sure how many,
    but basic research suggests it could be 2,000,000 FIB
    entries per 64MB RLDRAM unit.

via [<http://afnog.org/pipermail/afnog/2012-February/000161.html>](http://afnog.org/pipermail/afnog/2012-February/000161.html)

Functional Outline
------------------

The CFEB or CFEB-E provides the following functions:

-   Route lookups: Performs route lookups using the forwarding table stored in the synchronous SRAM (SSRAM) on CFEBs or stored in the RLDRAM on CFEB-Es.

<!-- -->

-   Management of shared memory: Uniformly allocates incoming data packets throughout the router's shared memory.

<!-- -->

-   Transfer of outgoing data packets: Passes data packets to the destination FIC or PIC when the data is ready to be transmitted.

<!-- -->

-   Transfer of exception and control packets: Passes exception packets to the microprocessor on the CFEB or CFEB-E, which processes almost all of them. The remainder are sent to the Routing Engine for further processing. Any errors originating in the Packet Forwarding Engine and detected by the CFEB or CFEB-E are sent to the Routing Engine using system log messages.

via [<http://www.juniper.net/techpubs/en_US/release-independent/junos/topics/concept/cfeb-m10i-description.html>](http://www.juniper.net/techpubs/en_US/release-independent/junos/topics/concept/cfeb-m10i-description.html)

[Category:Hardware](/Category:Hardware "wikilink") [Category:Components](/Category:Components "wikilink") [Category:Switch Boards](/Category:Switch_Boards "wikilink") [Category:Stub](/Category:Stub "wikilink") [Category:Image needed](/Category:Image_needed "wikilink")