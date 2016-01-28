---
title: Engine 4 Plus
permalink: /Engine_4_Plus/
---

The Engine 4+ linecard provided everything that was lacking in the standard Engine 4 release. It is labeled an "Enhanced Services" revision in some documentation rather than Engine 4+. This linecard can handle multiple features enabled at once except for a few corner cases where MPLS may not mix with some things such as uRPF. The RX+ ASIC provides a non-programmable high-speed ASIC with 25Mpps switching capacity. A vCAM (virtual cam) is used to provide ACLs, CAR and PBR.

A nice addition to the GSR family was the modular Gigabit Ethernet linecard nicknamed the "Tango". This linecard by default comes with 1 fixed Gigabit Ethernet interface and 3 EPA module slots. The EPA module has 3 interfaces for SFPs allowing you to populate 10 Gigabit Ethernet interfaces into one linecard. While this approach does work well in locations where you need to constantly add more Gigabit Ethernet interfaces as you grow, it has one drawback. The EPA modules are **not** hot-swappable and result in the operator having to remove the linecard to install an EPA module.

-   10Gbps connection to the fabric

<!-- -->

-   Engine 4+ - lists as **L3 Engine: 4 - Enhanced Services OC192/QOC48 (10 Gbps)**

| Card                                | Part Name (FRU)  | Part Number (MAIN) | Codename   |
|-------------------------------------|------------------|--------------------|------------|
| 1 Port 10-Gigabit Ethernet          | 1x10GE-LR-SC=    | 800-20017-09       | Ashara     |
| 10 Port Modular Gigabit Ethernet    | EPA-GE/FE-BBRD   | 800-15675-02       | Tango      |
| 1 Port OC192 POS SR - Edge Release  | OC192E/POS-SR-SC | 800-18668-06       | Unknown    |
| 1 Port OC192 POS IR - Edge Release  | OC192E/POS-IR-SC | 800-18670-02       | Unknown    |
| 1 Port OC192 POS VSR - Edge Release | OC192E/POS-VSR   | 800-19043-02       | Unknown    |
| 4 Port OC48 POS SR - Edge Release   | 4OC48E/POS-SR-SC | 800-18672-02       | Unknown    |
| 4 Port OC48 DPT                     | Unknown          | Unknown            | Dense-48   |
| 1 Port OC192 DPT                    | Unknown          | Unknown            | Merlin-192 |

[Category:GSR Linecards](/Category:GSR_Linecards "wikilink")