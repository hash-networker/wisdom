---
title: SUP720 CoPP
permalink: /SUP720_CoPP/
---

Control Plane Policing on the SUP720
====================================

Control Plane Policing (CoPP) is a mechanism for managing traffic which is directed towards the management plane of a Cisco router. The SUP720 supports hardware-assisted CoPP. This document explains how CoPP operates on a SUP720, what its limitations are, and how it interoperates with mls rate-limiting. A sample SUP720 CoPP configuration is provided with explanations.

This document was written as a summary of an extensive discussion of SUP720 CoPP on the [Cisco NSP](https://puck.nether.net/mailman/listinfo/cisco-nsp) mailing list.

SUP 720 Architecture
--------------------

Before reading this document, please read the SUP720 sections from the following URLs:

-   [Cisco Catalyst 6500/Cisco 7600 Series Supervisor Engine 720 Data Sheet](http://www.cisco.com/en/US/prod/collateral/switches/ps5718/ps708/product_data_sheet09186a0080159856_ps2797_Products_Data_Sheet.html)
-   [Cisco Catalyst 6500 Architecture White Paper](http://www.cisco.com/en/US/prod/collateral/switches/ps5718/ps708/prod_white_paper0900aecd80673385.html)
-   [Catalyst 6500/6000 Switch High CPU Utilization](http://www.cisco.com/en/US/products/hw/switches/ps708/products_tech_note09186a00804916e0.shtml)
-   [Catalyst 6500 Series Switches PFC, DFC, and CFC FAQ](http://www.cisco.com/en/US/products/hw/switches/ps708/products_qanda_item09186a00809a7673.shtml)

Definitions
-----------

### RP - The Route Processor

The RP is the primary CPU on the SUP720 card. It handles certain types of traffic, including host traffic to / from the box (i.e. logging in, snmp, etc). Under certain circumstances, traffic that would normally be forwarded in hardware is sent to the RP.

### Glean Traffic

Glean traffic is IP traffic for which there are no ARP entries in the ARP cache, and which need ARP resolution before they can be forwarded. Glean traffic is processed by the RP.

### Punt Traffic

Punt traffic is traffic which cannot be forwarded in hardware and needs to be sent (i.e. punted) to the RP CPU to be forwarded. In some ways, this is equivalent to what used to be called "process switched traffic".