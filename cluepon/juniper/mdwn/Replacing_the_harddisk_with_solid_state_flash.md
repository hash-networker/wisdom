---
title: Replacing the harddisk with solid state flash
permalink: /Replacing_the_harddisk_with_solid_state_flash/
---

Replacing the harddisk on a RE with solid state flash
-----------------------------------------------------

The harddisk on a RE (atleast 2.0/3/4/5) can be easily upgraded to solid state flash. This helps to mitigate mechanical failures of harddisks. Especially the M7i harddisks are known to have high failure rates.

Its recommended to use a fast SSD media, in particular one with SLC chips since they are alot faster than MLC versions. This helps speeding up the boot process.

You need a 2,5" (laptop style) IDE harddisk. For example: Transcend 8GB SLC: TS8GSSD25-S. The newer model TS32GPSD320 (32 GB in this example) will work as well, but you won't be able to tighten the SSD to the RE because of a tiny design flaw. Half of the SSD will be "in the air". Only use this model if you're feeling adventurous. Check www.truemetal.org/universe/blog/2013/03/upgrading-juniper-re-5-0-re-400-to-ssd for a picture of what it looks like.

KingSpec SSD 32 GB MLC (model CM3-KS-057-107 / KSD-PA25.1-032MJ) will work. But you have to play around with the jumper settings until you find slave or CS. The jumper settings are printed incorrectly on the SSD case (CS and slave is \*both\* "no jumper" - which is wrong)!

Note: You need to jumper the SSD to CS (cable select) \*or\* SLAVE. If the HDD is not detected on boot-up please try a different setting. Do not set to Master/Primary as this will cause a collision with the on-board compact flash card.

It is very likely that JunOS will not be able to use the SMART function of the SSD thus you will have errors in your logs. If this happens you need to turn off SMART:

Turning off SMART
-----------------

set system processes disk-monitoring disable

[Image:ressd.jpg](/Image:ressd.jpg "wikilink")