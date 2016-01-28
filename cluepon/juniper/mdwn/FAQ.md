---
title: FAQ
permalink: /FAQ/
---

-   Q: I know nothing about JunOS. Can you recommend some introductory reference materials?
-   A: Try the [instruction videos](http://www.juniper.net/training/elearning/junos_cli/index.html) on Juniper's site.

<!-- -->

-   Q: Can I insert my M20 PIC into an M5, or vice versa?
-   A: [PIC compatibility](/PIC_compatibility "wikilink")

<!-- -->

-   Q: Can I buy 3rd party RAM to upgrade my routing engine without going broke?
-   A: [Route Engine DRAM Compatibility](/Route_Engine_DRAM_Compatibility "wikilink")

<!-- -->

-   Q: Do I need an AS PIC to get flow stats from my router?
-   A: Not necessarily. You can sample and build flow records export base on RE. Unfortunately this method consumes RE CPU and competes with other RE tasks. So be careful when you choose the number of samples per second. Anyway there is a hard limit of a few 1000 of pps. Also the PFE CPU can be stressed by RE-based sampling and flow export. To be able to sample more pps you need AS-PIC or PM-PIC (monitoring PIC). Each of them has some limits, but they are much higher than is applicable for the RE.

<!-- -->

-   Q: Where can I obtain SMB cables for the DS3 / CHDS3 PIC?
-   A: eBay, or any cable manufacturer can make them, or you can get BNC to SMB adapters

<!-- -->

-   Q: I use common 735a cable and crimper die already. Who sells SMB connectors for 735a?
-   A: Trompeter/Semflex, but be prepared to pay $9/ea instead of $.90/ea for BNC connectors (Aside from being more expensive, these connectors are also much harder to put on 735a cable than BNC connectors. Like anything else, once you get past the learning curve, it becomes much easier)

<!-- -->

-   Q: Can I add a CF card to my M10i/M7i? Can I upgrade my flash disks to support JunOS 8.5/9.0 requirements?
-   A: Juniper sells a flash kit, or see [flash compatibility](/flash_compatibility "wikilink").

<!-- -->

-   Q: I am trying to upgrade from 8.2 (or below), but I'm told to upgrade to 8.3 or newer first, but the version I want isn't available and any newer code that is does not work, what do I do?
-   A: When upgrading from 8.2 or below you need to use the <span class="plainlinks">[<span style="color:black;font-weight:normal; text-decoration:none!important; background:none!important; text-decoration:none;">marc manoff</span>](http://www.marcdmanoff.com/) "no-validate" option.

Questions from \#juniper @ Freenode
-----------------------------------

-   Q: How can I see my interface MAC address?
-   A: show interfaces ***INTERFACE*** extensive | match Hardware

<!-- -->

-   Q: How do I show all routes originating from AS1234?
-   A: show route protocol bgp aspath-regex ".\* ***1234***"
