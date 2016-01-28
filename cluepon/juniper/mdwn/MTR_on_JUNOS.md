---
title: MTR on JUNOS
permalink: /MTR_on_JUNOS/
---

Since JUNOS 8.0 there is a new option which allows to run traceroute in a 'MTR-like' mode:

` user@juniper> traceroute monitor $DESTINATION`

where $DESTINATION is an IP or domain-name of the target host.

--

You can do a more intensive version of this through the shell. Requires root access

` user@juniper> start shell`
` % su`
` Password:`
` root@CR1% mtr -i 0.02 4.2.2.2`