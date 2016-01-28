---
title: Basics
permalink: /Basics/
---

Cisco develops various routers, switches, SONET MUXs, Firewalls, VPN concentrators and several other pieces of equipment and software related to telecommunications. This site is focused on the higher end of routing equipment specifically.

The following routers are covered:

-   Cisco CRS-1 - Codename: Q and 1/2 Q (for half rack deployments)

This router received much press as it was nicknamed HFR (in the tradition of the GSR being named BFR) by the press and most Cisco employees. It's goal is to allow large carrier/service-providers the ability to expand to a multi-chassis deployment as well as deliver an OC-768 interface. It operates the Cisco IOS-XR modular software and can operate with multiple route processors and switching fabrics.

-   Cisco GSR

This router, introduced in the late 1990s was Ciscos solution to many issues operators were facing with the scalability of the 7500 platform. Originally designed as both a router and an ATM switch, it provides redundant route processors and switching fabric. Each individual line card uses a distributed fowarding table and falls within specific line card engine types depending on the speed provided to the fabric as well as feature set.

-   Cisco 6500/7600

This switch/router has been increasing popular in deployments in the last several years. The Cisco 6500 was primarily used as a Layer-2 only switch with some routing functionality provided via the MSFC routing daughter card. Cisco has "re-invented" this product by releasing the 7600, which is very similar to the 6500. This product can now be as capable as the GSR on many levels with the Supervisor 720 module. Cisco seems to push the 6500 for more enterprise deployments while the 7600 as a service provider/carrier device.

-   Cisco 7500

An old horse of the Cisco fleet, which was once at the core of almost all Internet backbones at some point in time. While its speeds are dwarfed by many routers and switches on the market today, they are certainly not rare to come by on the open market. This model provided decent Ethernet, Fast Ethernet, channelized DS3, DS3 and even optical interfaces to the customer base. Cisco has released several additions onto this platform to increase the life of the product such as the newer VIP and RSP boards. While it cannot push packets as much as modern routers, its current low price guarantees it will have a place in some sort of network for at least a few years more.

------------------------------------------------------------------------

Cisco organizational structure

It helps in understanding certain parts of the Cisco organization to determine who you are dealing with.

Sales

-   AM - Account manager - This is your local sales guy. He may work with one specific account or with multiple accounts based on the sales organization he's in. He is tied to two things, margin and quota. He handles contractual issues, purchasing, and quotes.

<!-- -->

-   SE - Systems Engineer - SEs at Cisco usually do not do quotes (it depends on the org). SEs are there to provide a technical resource to the AM and the customer. SEs are dedicated in the same way the AM is dedicated but reports to a different manager. The SE does not ever report to the AM, he reports to the SEM. SEs at Cisco run from either Associate SE (like a Cisco Academy grad) to SE5 (4/5 only recently got added).

<!-- -->

-   SEM - Systems Engineering Manager - The SEM oversees either a single or multiple accounts usually paired with a Regional Manager (RM). The SEM isn't necessarily someone who has climbed the SE tree at Cisco, he may be hired from outside or have never gotten above SE2.

<!-- -->

-   CSE - Consulting Systems Engineer - A CSE is just like an SE except they work on strategic initiatives across multiple accounts. There are not only multiple designations (CSE1, CSE2, CSE3) of SEs, but there are multiple "levels" of CSEs (some exist for the entire service provider org, some exist for cable SPs, some exist for people in New York, etc, and some even exist for multiple accounts).

<!-- -->

-   CSA - A CSE assigned to a specific account who only does overall architecture/vision/strategy for that account. Not every account has this.

<!-- -->

-   DE - Development Engineer - These are individuals who are typically not permitted to speak with customers. They typically either work in an architecture, testing or development (code) role.

<!-- -->

-   TAC - Technical Assistance Center - The group that handles cases opened by customers. They typically will attempt to reproduce your issue in their own lab environment, and if a bug is found, filed and communicate with the DE.

<!-- -->

-   PSIRT - Ciscos internal security group that handles DDTS/bugs opened that impact the security of Cisco products. Any bugs involving PSIRT are typically locked to PSIRT employees only.

<!-- -->

-   NCE/AS - Network Consulting Engineers/Advanced Services - A service that can be purchased to provide customers with dedicated resources at Cisco to assist in issue resolution, future code bug scrubs and various other issues with Cisco. While expensive, they are skilled and knowledgeable people. NCEs exist as part of 4 programs

<!-- -->

-   NSSTG/ITD - IOS Technologies Division - This section of Cisco focuses on the writing, compiling and testing of the Cisco IOS. These folks are actually not the whole story since a lot of IOS-specific stuff happens in each business unit. NSSTG usually handles "core" protocols (MPLS, OSPF, BGP, etc) but they don't handle obscure things like switching in most cases.

<!-- -->

-   BU - Business Unit - One of the confusing things about Cisco - the different Business Units which produce (sometimes overlapping) products in different sectors

<!-- -->

-   CCMSBU - Carrier Core Multi-Service Business Unit - This BU handles the GSR, 10k, CRS-1. They also do all code development for 12.0(31)S and beyond for the GSR as well as IOS-XR.

<!-- -->

-   ERBU - Edge Routing Business Unit

<!-- -->

-   DSBU - Desktop Switching Business Unit - BU handling 2950 ... 3750 switches etc
