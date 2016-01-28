---
title: Netflow limitations
permalink: /Netflow_limitations/
---

Packet sampling capacity
------------------------

On JunOS based platforms packet sampling to the [Routing Engine](/Routing_Engine "wikilink") is rate limited to approximately 7,000 pps. This rate limit is designed to protect the RE from floods of traffic which could impair operation. Higher performance packet sampling is available on the [Monitoring PIC](/Monitoring_PIC "wikilink") or on the [AS PIC](/AS_PIC "wikilink").