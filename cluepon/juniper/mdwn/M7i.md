---
title: M7i
permalink: /M7i/
---

The [Juniper](/Juniper "wikilink") [M7i](/M7i "wikilink") ([Codename:](/codenames "wikilink") *Calvin*) router replaced Juniper's [M5](/M5 "wikilink") router. The M7i is a minor product refresh which decreased the cost of production and improved capacity (through the introduction of a [FIC](/FIC "wikilink")) and feature sets (through the introduction of onboard [Tunnel PIC](/Tunnel_PIC "wikilink") functionality and an optional onboard [Adaptive Services module](/Adaptive_Services_module "wikilink")). The primary goal of the M7i development was to make Juniper's low- to mid-range platforms more price competitive with the [Cisco](/Cisco "wikilink") 7200 series. __NOTOC__

Innovations
-----------

The M7i and [M10i](/M10i "wikilink") were the first routers released using the [ABC-Chip](/ABC-Chip "wikilink") chipset, and replaced the older [Martini architecture](/Martini_architecture "wikilink") based [M5](/M5 "wikilink") and [M10](/M10 "wikilink") respectively. The [ABC-Chip](/ABC-Chip "wikilink") reduced the cost of production significantly. The M7i also reduced the weight of the [M5](/M5 "wikilink") by about half.

Physical
--------

The M7i is 3.5" (2U) high, 17.5" wide (19" with rack ears), 18" deep, and weighs 36.5lbs when fully loaded.

On the RE-850 Routing Engine, there is a built-in 1x G/E, 1000 BASE interface. This shows up as **ge-1/3/0**

The PIC slots are then numbered dependent on hardware, *type-FPC/SLOT/INTERFACE* but if each PIC had a single ge interface:

-   ge-0/0/0
-   ge-0/1/0
-   ge-0/2/0
-   ge-0/3/0

**For those not so familiar with M-Series Juniper hardware, The PIC slots are numbered from RIGHT to LEFT**, so ge-0/0/0 is the furthest RIGHT, with ge-0/3/0 being the first LEFT.

A 4 x 10/100M PIC in the **left-most** PIC slot would look like:

-   fe-0/**3**/0
-   fe-0/3/1
-   fe-0/3/2
-   fe-0/3/3

With interface x/x/0 being the **right-most** port, and x/x/3 being the **left-most** port.

### Power

Both AC and DC [power supplies](/power_supplies "wikilink") are available for the M7i. The system requires one power supply for operation, with a second for redundancy. When two power supplies are operating, the load is shared across both. The maximum system power draw for a fully loaded configuration is around 274 watts. The AC supply supports input voltages in the range of 100-264 volts, while the DC supply supports from -36 to -72 volts.

### Cooling

The M7i is cooled by 4 fans proving side to side cooling with airflow from left to right. The fans are field replaceable but the router should not be operated without fans for more than a moment. The cooling requirements are met by 3 operating fans.

Routing Engine
--------------

The M7i originally shipped with [RE-5.0](/RE-400 "wikilink") (Celeron 400MHz, 256MB/512MB/768MB DRAM), but now has the [RE-5.0+](/RE-850 "wikilink") (Celeron 850MHz, 1536MB DRAM) available as an option.

Forwarding
----------

The M7i utilizes the [Calvin architecture](/Calvin_architecture "wikilink") which is largely the same as the [Martini architecture](/Martini_architecture "wikilink"). The Calvin architecture does have somewhat lower performance than the Martini (16Mpps vs 40Mpps) but the M7i does not have sufficient [PIC](/PIC "wikilink") slot capacity to approach this reduced limit.

Hot Swapping
------------

PICs, Fans, and Power Supplies are hot swappable.

[Category:Hardware](/Category:Hardware "wikilink") [Category:M-series](/Category:M-series "wikilink")