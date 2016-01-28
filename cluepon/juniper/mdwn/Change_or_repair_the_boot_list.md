---
title: Change or repair the boot list
permalink: /Change_or_repair_the_boot_list/
---

If you get something like this:

`--- JUNOS 7.5R1.12 built 2006-02-05 08:13:09 UTC`
`---`
`--- NOTICE: System is running on alternate media device  (/dev/ad1s1a).`
`---`

And see the following in the boot message file:

`DEVFS: ready to run`
`ad0: not attached, missing in Boot List`
`ad1: DMA limited to UDMA33, non-ATA66 cable or device`
`ad1: 28615MB `<FUJITSU MHT2030AT>` [58140/16/63] at ata0-slave UDMA33`
`Mounting root from ufs:/dev/ad1s1a`</code>`   `

Then you might like to try putting the compact flash (on the RE) back into the boot list. Of course it may be that the flash is actually faulty but I have seen this fix the problem in the past. As root in the shell you might see this:

`% sysctl machdep.bootdevs`
`machdep.bootdevs: pcmcia-flash,pcmcia-flash-1,disk,lan`
`%`

When it should look like this:

`% sysctl machdep.bootdevs`
`machdep.bootdevs: pcmcia-flash,pcmcia-flash-1,`***`compact-flash`***`,disk,lan`
`%`

Put the compact flash back in the list like this:

`sysctl -w machdep.bootdevs=pcmcia-flash,pcmcia-flash-1,compact-flash,disk,lan`

Note that the actual list will depend on the RE, so it pays to check on a similar RE that is working fine to be certain of the names and order of the boot devices.

[Category:Stub](/Category:Stub "wikilink")