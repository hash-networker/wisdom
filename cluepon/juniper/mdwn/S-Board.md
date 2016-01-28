---
title: S-Board
permalink: /S-Board/
---

On Juniper M-series routers, packet forwarding functionality was implemented on the so-called S-boards which typically hosted L3 lookup ASIC, service processor and glue logic. Since different M-series platforms had different functionality, this led to an array of S-board names:

| Platform                                        | Name                     | Function  |                                                                                                                                                |
|-------------------------------------------------|--------------------------|-----------|------------------------------------------------------------------------------------------------------------------------------------------------|
| [M40](/M40 "wikilink")                          | [SCB](/SCB "wikilink")   | L3 lookup | Originally shipped with [Internet Processor](/Internet_Processor "wikilink"), later [Internet Processor II](/Internet_Processor_II "wikilink") |
| [M20](/M20 "wikilink")                          | [SCB](/SCB "wikilink")   | L3 lookup | Extended M40 SCB design for "cold spare" redundancy of the forwarding plane                                                                    |
| [M160](/M160 "wikilink")                        | [SFM](/SFM "wikilink")   | L3 lookup | Allowed for OC192 support via packet spraying across four [PFEs](/PFE "wikilink")                                                              |
| [M40e](/M40e "wikilink")                        | [SFM](/SFM "wikilink")   | L3 lookup | Utilizes M160-style packet distribution for [PFE](/PFE "wikilink") redundancy                                                                  |
| [M5](/M5 "wikilink")/[M10](/M10 "wikilink")     | [FEB](/FEB "wikilink")   | L2&L3 lkp | Single board integrating all three ASICs of [ABC chipset](/ABC_chipset "wikilink")                                                             |
| [M7i](/M7i "wikilink")/[M10i](/M10i "wikilink") | [cFEB](/cFEB "wikilink") | L2&L3 lkp | Single board featuring single integrated ABC ASIC                                                                                              |
| [M120](/M120 "wikilink")                        | [FEB](/FEB "wikilink")   | L2&L3 lkp | Next-gen distributed M-series linecard based on [I-chip](/I-chip "wikilink")                                                                   |

