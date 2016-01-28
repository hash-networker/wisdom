---
title: Flash compatibility
permalink: /Flash_compatibility/
---

Adding a flash to [M7i](/M7i "wikilink") and [M10i](/M10i "wikilink")
---------------------------------------------------------------------

The base configurations of the [calvin architecture](/calvin_architecture "wikilink") boxes do not come with any flash. Juniper sells an optional flash kit which includes a 256M flash card and a CF to PCMCIA converter. The PCMCIA is only used for doing router recovery. The flash card juniper ships is just a plain sandisk (part number ??) flash card with a Juniper sticker on it. Another flash card may be substituted, but Juniper will not support this configuration.

Upgrading existing flash disks
------------------------------

Older routing engines come with smaller flash disks (e.g. 128 MB). Some versions of [RE-3.0](/RE-3.0 "wikilink") come with 128 MB; others with 256 MB. JunOS 8.5 requires 256 MB. JunOS 9.0 requires 512 MB flash.

It's possible to upgrade flash disks with commodity equipment (at least all SanDisk CF cards are believed to work, but see below) or buy an upgrade kit from Juniper.

However, there is one caveat: if the flash has too high performance (e.g. theoretical 20 MB/s read/write instead of slower ones that Juniper uses, reportedly around 5 MB/s range), this can cause some unsupported issues. In particular, when you write a software image on the flash on a router (using 'dd' command), the heavy block I/O consumes all CPU, and the routing protocol sessions (and similar tasks requiring CPU time) will drop when writing the flash. This was observed on "SanDisk 1GB CompactFlash Extreme III" cards but is believed to apply to many other models as well.

So, if you buy commodity flash disks, don't write images on them on production routers!

More information can be found here: [Replacing/upgrading_the_CF_on_a_RE](/Replacing/upgrading_the_CF_on_a_RE "wikilink")