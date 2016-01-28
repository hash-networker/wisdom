---
title: PIC hotswap
permalink: /PIC_hotswap/
---

It is possible to hot swap PICs in most Juniper routers, though in the M20 and M40 it is physically difficult to do so.

Before hot swapping a PIC it is necessary to 'offline' the PIC using either the CLI (request chassis pic offline fpc-slot x pic-slot y) or a front panel button.

The PIC can then be removed, and replaced with either the same or a different type.

Once the new PIC has been inserted it must be brought online, again using either the CLI or front panel button.

Note that on 'traditional' M series routers (i.e. up to and including the M160) there will be an interruption to the packet forwarding across the FPC when a PIC is brought offline, due to the system restructuring the internal memory to account for this.