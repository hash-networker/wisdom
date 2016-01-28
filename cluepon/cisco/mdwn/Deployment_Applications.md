---
title: Deployment Applications
permalink: /Deployment_Applications/
---

Information
===========

-   [Portable Product Sheets - including Router Performance, Memory, Voice Density, etc.](http://www.cisco.com/web/partners/tools/quickreference/)

Smallest router for 2 full BGP feeds
====================================

Cisco 1841 with sec/k9 or advanced routing (for BGP) and 384MB RAM, 64MB flash. It works but you didn't specify throughput requirements. I would guess that the 1841 could handle two 10Mbps connections.

Smallest VPN/OSPF capable router
================================

Requirement :

We need a the cheapest (offcourse) router/firewall with 2 WAN interfaces (or trunkable WAN) that has accellerated VPN and runs OSPF. 2 tunnels accellerated is enough with 20-100Mbit throughput.

''' Options '''

-   2801 with AES128/SHA1/GRE encapsulating large packets and no AIM will come in around 20mbit/s (so you can infer that 1841 won't be quite enough), but with the AIM-VPN will bump up to around 40.
-   If you are planning any sort of routing or QoS or filtering, something a bit more beefy might be wise. If you are going up to 100mbps, something like a 2851 or 3825 minimum, or a 3845 for some headroom.

DSL QoS for client site routers
===============================

[Deployment_Applications/DSL_CPE_QoS](/Deployment_Applications/DSL_CPE_QoS "wikilink")