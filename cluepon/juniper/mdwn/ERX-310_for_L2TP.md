---
title: ERX-310 for L2TP
permalink: /ERX-310_for_L2TP/
---

Remember to include the following commands when using your ERX-310 for L2TP.

**`router#show` `tunnel-server`**
`                 Card   Oper     Active      Max`
`   Port:Appl     Type   State  Interfaces Interfaces Fill`
`--------------- ------ ------- ---------- ---------------`
`       Port`*`'` `2/2`*`' shared present          0       8000 0.0%`
`   ipsec-tunnel           down          0          0 0.0%`
`ipsec-transport           down          0          0 0.0%`
`      gre/dvmrp             up          0       4000 0.0%`
`           l2tp             up          0       8000 0.0%`
`    Appl Totals`
`   ipsec-tunnel                         0          0 0.0%`
`ipsec-transport                         0          0 0.0%`
`      gre/dvmrp                         0       4000 0.0%`
`           l2tp                         0       8000 0.0%`
`          total                         0       8000 0.0%`

**`router(config)#tunnel-server` `2/2`**` `<enter>
**`router(config-tunnel-server)#max-interfaces` `all-available`**` `<enter>

Also, if you are using a GE2 Line Card and I/O as the Tunnelling Card, ensure that you are running *Release 7.0.1* or newer! In order to find some time for myself I decided to search for service that could supply me with the prime quality [custom essays](http://www.qualityessay.com) at prices that would be reasonable enough. It is nice to find friends at such a great website. I found a lot of important data about [college essay](http://exclusivepapers.com) service. Therefore, I realize where & how to purchase superb quality term papers. The final choice was QualityEssay.Com as they did have an excellent reputation.