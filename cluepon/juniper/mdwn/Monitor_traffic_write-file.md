---
title: Monitor traffic write-file
permalink: /Monitor_traffic_write-file/
---

It is possible to record a [tcpdump](/tcpdump "wikilink")-format copy of packets monitored via the "monitor traffic" command, using the hidden command "monitor traffic write-file <filename>". Conversely, existing files written this way can be replayed into [tcpdump](/tcpdump "wikilink") ASCII format using the command "monitor traffic read-file <filename>".

These commands are hidden due to concerns about users accidentally writing large files to the /var partition, causing a lack of disk space and potentially impairing the operation of the router.

Examples
--------

`user@juniper> monitor traffic write-file testing interface ge-0/0/1   `
`Listening on ge-0/0/1, capture size 96 bytes`
`^C`
`107 packets received by filter`
`0 packets dropped by kernel`
`user@juniper> monitor traffic read-file testing `
`21:12:52.973693  In arp who-has Ash-Equinix-GigE.aleron.net tell equinix-ashburn.pch.net`
`21:12:52.974310  In arp who-has Ash-Equinix-GigE.aleron.net tell equinix-ashburn.pch.net`
`21:12:53.531134  In arp who-has exchange-cust1.ash.equinix.net tell g1-1.border1.eqx.iad.transedge.com`
`21:12:53.532208  In arp who-has exchange-cust1.ash.equinix.net tell g1-1.border1.eqx.iad.transedge.com`
`21:12:53.780775  In arp who-has Ash-Equinix-GigE.aleron.net tell ge2-5.fr1.iad.llnw.net`
`21:12:53.781676  In arp who-has Ash-Equinix-GigE.aleron.net tell ge2-5.fr1.iad.llnw.net`
`21:12:53.796944  In arp who-has 64.200.163.132 tell WASHDC5LCE1.3.0.wcg.net`
`21:12:53.797166  In arp who-has 64.200.163.132 tell WASHDC5LCE1.3.0.wcg.net`
`21:12:53.964604  In arp who-has equinix-peering.lumison.net tell equinixexchange.realconnect.com`
`21:12:53.965656  In arp who-has equinix-ashburn.novia.net tell equinixexchange.realconnect.com`
`21:12:54.531061  In arp who-has equinix-peering.lumison.net tell g1-1.border1.eqx.iad.transedge.com`