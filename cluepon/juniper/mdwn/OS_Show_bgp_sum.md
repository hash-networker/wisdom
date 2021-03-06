---
title: OS Show bgp sum
permalink: /OS_Show_bgp_sum/
---

Information
-----------

This Op Script shows bgp summary information with single line for each peer (for "| match ..." working), with bgp group and description information.

Usage
-----

Config it:

Put the file `show-bgp-sum.slax` to `/var/db/scripts/op` directory, then configure `set` `system` `scripts` `op` `file` `show-bgp-sum.slax`

Call it:

`gul@quoll> op show-bgp-sum `
`   Peer                    AS  Last Up/Dwn State         Act/Rcv/Acc    Group        Descr`
`62.149.20.73             15497      6w2d7h Establ                8/8/8 CL-W-F       Colocall`
`77.47.128.1              25500     2w0d22h Establ                0/1/1 CL-W-FD      Kpi`
`80.81.192.11              5413      6w2d0h Establ          117/179/124 PEERING      Gx-networks`
`80.81.192.19              5466      6w2d7h Establ             29/38/29 PEERING      Eircom`
`80.81.192.24             41692      6w2d7h Establ          117/143/135 PEERING      OpenCarrier`
`80.81.192.28             20940      6w2d7h Establ                2/6/2 PEERING      Akamai`
`80.81.192.81              5588      6w2d7h Establ       2352/2506/2504 PEERING      Gtsce`
`80.81.192.115            10310      6w2d0h Establ          173/188/173 PEERING      Yahoo`
`80.81.192.172             6939      6w2d7h Establ    12078/13476/13476 PEERING      Henet`
`80.81.192.157             6695     7:01:40 Establ    18623/45692/45692 PEERING      DE-CIX #1`
`195.219.148.73            6453      6w2d2h Establ 253807/314338/314338 TATA         Tata`
`213.133.164.166          15169     4w5d13h Establ          186/186/186 GOOGLE       Google`
`2001:5a0:2800::1d         6453      6w2d2h Establ       1649/2651/2651 TATA         Tata`
`2a02:2d8:0:2805:232a::    9002      6w1d3h Establ       1142/2778/2778 RETN         RETN`

Bugs
----

Show configuration cannot display both commit script results and groups inheritance together (PR 519304). So, now transient changes are ignored, and groups will not be displayed if such part of configuration is a result of commit-script. If it's important, there is a possible workaround, but it can only be used by users that have access to edit the full configuration, and only when there is no pending candidate configuration changes that need to be committed. In this case, I suppose, time of the script processing will be increased. I hope this workaround is not needed.

If there are peers with the same ip addresses in different route-instances, the script will not display instance/group information correctly.

ChangeLog
---------

0.1 - Initial Version
0.2 - Process groups inheritance, support route-instances