---
title: Set protocols bgp minimum-hold-time
permalink: /Set_protocols_bgp_minimum-hold-time/
---

In [JunOS](/JunOS "wikilink") 7.6R1 the minimum allowable BGP hold timer was changed from 6 to 20 seconds. This hidden knob was added to allow the hold time to be altered below this minimum. Your config will be unsupported by JTAC, but if you are running a mixed-vendor environment you may need to use this for compatibility with the existing network.