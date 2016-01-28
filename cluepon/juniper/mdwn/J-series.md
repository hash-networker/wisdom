---
title: J-series
permalink: /J-series/
---

[thumb|right|Older Juniper J-series Routers](/Image:J-Series_Front.jpg "wikilink") [thumb|right|Newer Juniper J-Series Routers](/Image:New_J-Series.png "wikilink")

J-Series (internal codename Pepsi) is a line of low-end routers launched by Juniper during telecom recession period (2003-2004). It became the first Juniper product to feature forwarding plane with partial software assist. The idea behind J-Series was to utilize the rapidly improving performance of general-purpose x86 CPUs and commercial mainboard designs. Hardware packet forwarding assistance was provided by Intel IXP (and later Cavium) network processors, which allowed for considerable speed advances compared to "traditional" software-only routers based on RISC processors.

J-series runs a "diet" (slightly cut-down) version of JunOS (missing a few features such as Logical Routers). All J-series systems have the ability to have recovery/upgrade images and configurations pulled off USB flash drives from front-mounted USB port.

Earlier J-Series products had a complex licensing regime, where individual ports would be unlocked with a software license, and some features such as DLSw were add-on licenses. Changes in 2006 and 2007 have meant that only "advanced BGP" (route-reflector server) and J-Flow features still require licensing.

Newer J-Series routers contain a TDM-backplane, and support a range of Avaya voice equipment, including a locally-survivable media gateway, ISDN BRI, T1 voice and FXO/FXS analogue voice ports. Currently these cards are managed with a local CLI, not via JUNOS, and they are only available from Avaya, not direct from Juniper.

Newer J-Series routers also share common hardware platforms with the SSG series of UTM firewalls - J2320 is SSG320M, J2350 is SSG350M, J4350 is SSG520M and J6350 is SSG550M. A seperate license of the software image can be used to switch between the two operating systems.

According to the marketing material Juniper says that the J-series routers support VLANs (Advanced Switching). What they fail to tell you is that VLAN's are not supported on the onboard NIC, and you can only enable VLAN's on 1 uPIM module at a time. This is because the Advanced Switching is done on the uPIM module, not on the router mainboard.

Older J-Series Products (J4300 and J6300 are EOS)

-   [J6300](/J6300 "wikilink")
-   [J4300](/J4300 "wikilink")
-   [J2300](/J2300 "wikilink")

Newer J-Series Products

-   [J6350](/J6350 "wikilink") -- October 2006
-   [J4350](/J4350 "wikilink") -- October 2006
-   [J2350](/J2350 "wikilink") -- August 2007
-   [J2320](/J2320 "wikilink") -- August 2007

[Category:Hardware](/Category:Hardware "wikilink") [Category:J-series](/Category:J-series "wikilink") [Category:Stub](/Category:Stub "wikilink")