---
title: ATM PIC
permalink: /ATM_PIC/
---

[thumb|right|ATM PICs](/Image:ATM_PIC.jpg "wikilink")

A note relating to ATM1 PICs, particularly with a large number of VCs.

ATM1 PICs share transmission buffers between all ports on the card - this means all physical ports and all VCs. Sadly, there is no mechanism in place by default to prevent a single PVC, if exceeding its egress rate shape, from consuming all transmit buffers on the entire card. Which means that egress traffic on other VCs, if not within shaping profile, will be dropped straight away.

A workaround is to configure a finite queue length on each PVC, like so.

    thomas@NCT_M02# show interfaces at-0/1/1 unit 220
    description "ATM Circut XXX";
    encapsulation atm-snap;
    vci 0.220;
    shaping {
        vbr peak 40m sustained 40m burst 32;
        queue-length 50;
    }
    oam-liveness {
        up-count 2;
        down-count 5;
    }
    family inet {
        address 192.168.1.1/30;
    }

There are a total of 16k buffers per car. The ideal queue length can be calculated by:

16,382 / (MTU / 480)

This could be considered good practice anytime you have more than a few VCs on an ATM PIC. More info on this can be found [here](http://www.juniper.net/techpubs/software/junos/junos73/swconfig73-interfaces/html/interfaces-atm-config30.html).

[Category:Hardware](/Category:Hardware "wikilink") [Category:PICs](/Category:PICs "wikilink") [Category:Stub](/Category:Stub "wikilink")