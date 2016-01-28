---
title: Ios sxi
permalink: /Ios_sxi/
---

On Nov 13th, the 12.2(33)SXI software was released for the 6500 platform.

This page aims to document what works and what doesn't

What works
----------

-   MPLS
    -   IPv4 vrfs - tested & working, interop with 6500s running 12.2(18)SXF10, JunOS 8.3
        -   unicast - works
            -   including SNMP arp table i.e. ipNetToMedia
            -   including netflow
        -   multicast (MVPN) works
    -   IPv6 vrfs - works, tested against JunOS 8.3 [6vPE config](/ios_sxi/6vpe "wikilink")
        -   requires "mls ipv6 vrf" global config command
        -   unicast - works
            -   including SNMP neigh table i.e. cInetNetToMediaPhysAddress
            -   including Netflow from VRF interfaces (assuming Netflow v9 is used)
        -   NOTE: DHCPv6 relay does not work on VRF interfaces, TAC case opened
    -   ldp works
    -   l2vpn (pseudowire/xconnect) tested and works including interop with JunOS

<!-- -->

-   IPv4 (non-VRF/MPLS)
    -   All seems to work

<!-- -->

-   IPv6 (non-VRF)
    -   DHCPv6 relay works (but not in a VRF)
    -   HSRPv6 works
-   CLNS/ISIS
    -   works including v4+v6
-   BGP
    -   works including v4+v6

<!-- -->

-   OSPFv2
    -   interop with 6500 running 12.2(18)SFX10

<!-- -->

-   Management
    -   SCP works - no crash bug as per SXH3/3a
    -   SNMP works - can do snmp full walk, all expected MIBs present

What doesn't
------------

-   DHCPv6 in a VRF (TAC case opened)

Other Issues
------------

-   The mac aging timer appears to both:
    -   have changed by default from 300 to 480
    -   cannot be set lower than 480
    -   message logged on the console says:

<!-- -->

    % Vlan Aging time not changed since OOB is enabled and requires aging time to be atleast 3 times OOB interval - default: 480 seconds