---
title: Enabling packet based forwarding
permalink: /Enabling_packet_based_forwarding/
---

Force a recent JunOS (formerly JunOS ES) on SRX or J-series router to use packet based, rather than flow based, forwarding.

The advantage is that you can end up with a smaller and simpler configuration which will work more like a M or MX series.

The downfall is that any services configured under the 'security' stanza is no longer possible. This includes stateful-firewalls, nat, IPSec, etc!

    delete security
    set security forwarding-options family mpls mode packet-based
    set security forwarding-options family iso mode packet-based
    set security forwarding-options family inet6 mode packet-based