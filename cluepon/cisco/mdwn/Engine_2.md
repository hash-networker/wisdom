---
title: Engine 2
permalink: /Engine_2/
---

The Engine 2 linecard introduced a new set of unique problems to operators. The PSA (Packet Switching ASIC) microcode allowed operators to load specific bundles of features onto the linecard depending on what features were enabled on the router in general. There were three main problems with this architecture:

-   If you had an E2 line card in your router and you outbound ACLs on a different engine type **anywhere** else on the router, **all** Engine 2 linecards would implement an inbound ACL, resulting in a change in the microcode bundle and disabling potentially other features enabled on the E2's.

<!-- -->

-   Microcode bundles only permitted specific feature sets to be enabled on a line card at a time. For example, for several years you could not mix uRPF and Netflow on the same line card. This was later fixed in 12.0(23)S2. Still today there are similar situations where a microcode bundle does not support the specific feature set you would like to have enabled at a time.

<!-- -->

-   Each microcode bundle had a set priority that the operator could not override. Simply enabling a feature could result in changing microcode bundles (resulting in dropped packets in the process to switch the bundle).

The 1 Port OC48 POS line card was also known to be used as a "Tunnel Server" card. This would permit service providers such as Sprint (who did not want to enable MPLS but rather L2TPv3 for L3VPNs) to perform GRE encapsulsation/de-encapsulation (as GSRs do not support GRE by default). This process burned an entire linecard/slot for the tunneling processing.

Other complaints about this linecard is the 3 Port Gigabit Ethernet (Trident), this linecard clearly was oversubscribed and the "3rd" Gigabit Ethernet port would never be able to do line rate.

ACLs for example, have a drastic impact on performance:

-   128 line input ACLs - 800kpps
-   128 line output ACLs - 675kpps
-   448 line input ACLs - 690kpps
-   448 line output ACLs - 460kpps

<!-- -->

-   2.5Gbps connection to the fabric

<!-- -->

-   Engine 2 - lists as **Engine: 2 - Backbone OC48 (2.5 Gbps)**

| Card                                                                      | Part Name (FRU)         | Part Number (MAIN)       | Codename  |
|---------------------------------------------------------------------------|-------------------------|--------------------------|-----------|
| 4 Port OC12 POS IR                                                        | 4OC12/POS-IR-SC-B       | 800-5491-03,800-12143-01 | Unknown   |
| 8 Port OC3 POS                                                            | 8OC3/POS-SM             | 800-5599-02              | Scepter   |
| 16 port OC3 POS                                                           | 16OC3/POS-SM            | 800-5413-01              | Scepter   |
| 4 Port OC12 ATM                                                           | 4OC12/STM-4 MM-SC ATM   | 800-05669-03             | Inuit     |
| 8 Port OC3 ATM                                                            | 8OC03/ATM/TS-IR-B       | 800-20641-02             | Taz       |
| 3 Port Gigabit Ethernet                                                   | 3GE-GBIC-SC             | 800-6376-01              | Trident   |
| 1 Port OC48 DPT                                                           | OC48/STM-16 SRP-SR-SC-B | 800-05619-05             | Merlin-48 |
| 1 Port OC48 POS SR                                                        | OC48E/POS-SR-SC-B       | 800-5271-02,800-12135-01 | Unknown   |
| 1 Port OC48 POS LR                                                        | OC48E/POS-LR-SC-B       | 800-5414-01              | Unknown   |
| 1 Port OC192 POS - linecard with OC192 optics but 2.5Gb fabric connection | OC192R0/POS-SR-SC       | Unknown                  | Comet     |

[Category:GSR Linecards](/Category:GSR_Linecards "wikilink")