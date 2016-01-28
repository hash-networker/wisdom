---
title: Juniper architecture and interface counters
permalink: /Juniper_architecture_and_interface_counters/
---

Juniper's architecture processes and removes link-layer headers on the media-specific [PICs](/PIC "wikilink") before the packet ever reaches the rest of the [PFE](/PFE "wikilink"). Before the [Q-Chip](/Q-Chip "wikilink") based [PICs](/PIC "wikilink"), there was no way to do any analysis or accounting of these link-layer headers. This presents several challenges and changes in behavior which need to be taken into account.

The SNMP specification calls for the ifInOctets and ifOutOctets MIBs to include link-layer headers in their counters. Most platforms from other vendors do this, but [Juniper](/Juniper "wikilink") can not. This may result in data mismatches and potential incorrect analysis when comparing Juniper counters with those from other vendors.

This restriction is also what prohibits Ethernet MAC accounting and filtering on pre [Q-Chip](/Q-Chip "wikilink") PICs, and what caused [Tcpdump expressions](/Tcpdump_expressions "wikilink") issues in earlier [JunOS](/JunOS "wikilink") code.