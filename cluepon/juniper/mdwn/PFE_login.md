---
title: PFE login
permalink: /PFE_login/
---

To log in to the micro-kernel running on a switch card or FPC, use the following command from the CLI:

`user@router> start shell pfe `<network|direct>` `<component>

Or this command from the shell, as root:

`root@router% vty `<component>

You can use the "tnpdump" command to get a component to id mapping from the shell:

`root@router% tnpdump`

You can also use the cty command to connect to the FPC or DPC on the internal console port (useful for seeing the FPC boot sequence):

`root@router% cty `<component>

Example:
--------

`root@router% vty sfm0`
`SFM platform (266Mhz PPC 603e processor, 64MB memory, 512KB flash)`
`SFM0(router vty)# show version`
`Juniper Embedded Microkernel Version 7.2R1.7`
`Built by builder on 2005-04-22 01:32:02 UTC`
`Copyright (C) 1998-2005, Juniper Networks, Inc.`
`All rights reserved.`
`SFM platform (266Mhz PPC 603e processor, 64MB memory, 512KB flash)`
`Current time   : Aug 20 19:09:20.26201`
`Elapsed time   :      111+12:21:37`

This works on J-series fwdd too:
--------------------------------

`root@j2320> start shell pfe network fwdd    `
`BSD platform (Pentium processor, 424MB memory, 16384KB flash)`
`FWDD(j2320 vty)# sh threads`
`PID PR State     Name                   Stack Use  Time (Last/Max/Total)`
`--- -- -------   ---------------------  ---------  ---------------------`
` 1 H  asleep    Maintenance            664/32768  0/0/0 ms`
` 5 L  asleep    Sheaf Background       696/32768  1/1/23 ms`
`35 L  asleep    Cattle-Prod Daemon    2356/32768  0/0/0 ms`
` ...`

TCAM ACL (firewall)
-------------------

You can check TCAM ACL usage as well (from j-nsp):

Drop into the fpc shell from root, like so:

`RE:0% vty fpc0`
`BSD platform (MPC 8544 processor, 48MB memory, 0KB flash)`
`PFEM0(vty)# `

Next you need to find the vendor ID for the platform, like so:

`PFEM0(vty)# show tcam vendor    `
`Vendor = internal_ch3_tcam Vendor_id = 1`

For EX8200 it's vendor id 6, for EX3200 it seems to be vendor id 1.

Then you need to find the instance ID for the hardware you're looking for. On EX8200 I know instance 2 is used for GE cards, instance 4 is used for XE cards. On EX3200 there only seems to be instance 2 (as you'd expect):

`PFEM0(vty)# show tcam vendor 1 instances    `
` Vendor         Instance        Page Size`
`--------------------------------------------`
` internal_ch3_tcam         2         4 `

So then to view the usage info for this vendor/instance:

`PFEM0(vty)# show tcam vendor 1 instance 2 rules    `
`Number of rules as Ingress PACL: 0`
`Number of rules as Ingress VACL: 0`
`Number of rules as Ingress RACL: 528`
`Number of rules as   Egress PCL: 135`
`528 Ingress RACL rules`
`HW-index    Page_id    Entry_id    rule_size         fw_id    Rule`
`--------------------------------------------------------------------------------`
`    6296       1574           0            2            27    AUTOFW-INVALID-PROTOCOLS.ext.0`
`    6298       1574           2            2            27    AUTOFW-INVALID-PROTOCOLS.ext.1`
`    6496       1624           0            2            27    AUTOFW-BORDER-FILTERED-PROTOCOLS.ext.0`
`    6498       1624           2            2            27    AUTOFW-BORDER-FILTERED-PROTOCOLS.ext.1`
`    6708       1677           0            2            27    AUTOFW-BORDER-LIMIT-IP-OPTIONS.ext.0`
`    6710       1677           2            2            27    AUTOFW-BORDER-LIMIT-IP-OPTIONS.ext.1`
`    6960       1740           0            2            27    AUTOFW-LIMIT-ICMP-ECHO.ext.0`
` `
`TCAM utilization: 1326(used), 12938(free), 14264(total)`

And there is your total tcam utilization above. Depending on code and platform it may show you a slightly different view, for example here is the utilization on an EX8200 running older 10.1 code:

`PFEM15(vty)# show tcam vendor 6 instance 4 rules    `
`Instance 4`
`  DB 0      Ingr PACL:        0/     996 (current/max) rules. Util. 0.000%`
`  DB 1      Ingr VACL:        0/   12288 (current/max) rules. Util. 0.000%`
`  DB 2      Ingr RACL:      410/   32768 (current/max) rules. Util. 1.251%`
`  DB 3       Egr PACL:        0/    1024 (current/max) rules. Util. 0.000%`
`  DB 4       Egr PCL1:      103/    8188 (current/max) rules. Util. 1.258%`
`                                                                            `

[Category:JunOS](/Category:JunOS "wikilink")