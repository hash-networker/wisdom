---
title: References to upcoming products in JUNOS
permalink: /References_to_upcoming_products_in_JUNOS/
---

In JUNOS there are a couple of ways to find references to upcoming hardware releases. The first is the "display detail" modifier. For example, this is the output from JUNOS 10.1R2.8 running on an srx100:

`admin@srx100.lab# show chassis | display detail `
`##`
`## chassis: Chassis configuration`
`## require: interface `
`##`
`##`
`## aggregated-devices: Aggregated devices configuration`
`##`
`aggregated-devices {`
`   ##`
`   ## ethernet: Aggregated device options for Ethernet`
`   ##`
`   ethernet {`
`       ##`
`       ## device-count: Number of aggregated Ethernet devices`
`       ## range: 1 .. 128`
`       ##`
`       device-count 5;`
`       ##`
`       ## lacp: Global Link Aggregation Control Protocol configuration`
`       ## products: m5, m10, m20, m40, m160, t640, t320, m40e, TX Matrix, ggsn-c, m320, m7i, m10i, j2300, j4300,`
`j6300, m120, mx960, j4350, j6350, jsr2300, jsr4300, jsr6300, jsr4350, jsr6350, j2320, j2350, jsr2320, jsr2350, mx480,`
`sumatra, ex3200-24t, ex3200-24p, ex3200-48t, ex3200-48p, ex4200-24f, ex4200-24t, ex4200-24p, ex4200-48t, ex4200-48p,`
`ex8208, ex8216, mx240, txp, srx210-lm, srx210-hm, srx210-poe, srx210h-p-m, srx240-lm, srx240-hm, srx240-poe,`
`srx240h-p-m, srx630, srx650, srx680, srx100-lm, srx100-hm, srx100-wl-lm, srx100-wl-hm, srx100-vdsl-lm, srx100-vdsl-hm,`
`srx100-wl-vdsl-hm, ex2200-24t-4g, ex2200-24p-4g, ex2200-48t-4g, ex2200-48p-4g, ln1000-v, fx1600_48, fx2160_48`
`       ##`
`       lacp {`
`           link-protection;`
`       }`
`   }`
`}`

You can see references to the as yet unreleased SRX630 and SRX680 models. You can also see the [Google caches of their AV update pages](http://www.google.com/search?q=juniper+srx630).