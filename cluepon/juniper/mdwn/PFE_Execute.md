---
title: PFE Execute
permalink: /PFE_Execute/
---

Execute a command in the micro-kernel running on a switch card or FPC, use the following command from the CLI:

`user@router> request pfe execute target `<component>` command "`<command>`"`

Example:
--------

`user@router> request pfe execute target fpc0 command "show version"`
`SENT: Ukern command: show version`
`GOT:`
`GOT:`
`GOT: Juniper Embedded Microkernel Version 10.4R8.5`
`GOT: Built by builder on 2011-11-19 06:47:53 UTC`
`GOT: Copyright (C) 1998-2011, Juniper Networks, Inc.`
`GOT: All rights reserved.`
`GOT:`
`GOT:`
`GOT: ADPC platform (1200Mhz MPC 8548 processor, 1024MB memory, 512KB flash)`
`GOT: Current time   : Feb  9 14:02:52.795897`
`GOT: Elapsed time   :      8+20:29:54`
`LOCAL: End of file`

[Category:JunOS](/Category:JunOS "wikilink")