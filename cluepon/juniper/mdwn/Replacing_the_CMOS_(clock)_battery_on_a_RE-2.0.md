---
title: Replacing the CMOS (clock) battery on a RE-2.0
permalink: /Replacing_the_CMOS_(clock)_battery_on_a_RE-2.0/
---

Replacing the CMOS battery on a RE
----------------------------------

I had an older RE-2.0 aka RE-333 that had been sitting for a while. When it was powered on again it wouldn't keep it's clock date/time setting if the power was lost. Sounds like the CMOS battery was dead.

I haven't found any reference to the existence of any kind of CMOS battery on a RE-2.0 anywhere in Juniper's KB or techpubs, much less any reference to replacing one, so we are clearly in un-documented, "your mileage may vary" land. Disclaimers aside, this wasn't particularly hard, so lets get started.

The battery is located "under" the CPU heatsink area. It is not directly affixed to the heatsink for temperature dissipation or anything, but the CPU heat-sink extends over the battery area of the circuit board (with some vertical clearance room) such that it is difficult to remove the battery without removing the heatsink first. The easiest way to get the heatsink off is to remove the daughter board and the front (back?) panel as well. So, pretty much everything..

Start with the routing-engine flipped upside down. Remove all of the indicated screws on the bottom. Then, lift the plastic covering off and remove the two additional screws that are now uncovered (not pictured). These two screws that are hidden under the plastic covering will enable you to remove the top (daughter) board later on. [<File:Re_bottom.jpg>](/File:Re_bottom.jpg "wikilink")

Next flip the board right side up again and remove the two screws on the routing-engine "backplate" (is there a better term for this?) next to the "JUNOS PC CARD LABEL THIS WAY" text. When you have removed these screws you should be able to gently pry the backplate off of the routing engine - be careful not to let the backplate catch on the PCMCIA PC Card ejector handle when you remove it. Set the backplate aside for now. [<File:backplate.jpg>](/File:backplate.jpg "wikilink")

Now, on the top of the RE over by the RAM slots and hard disk, remove the screw indicated in the picture below. This screw will allow the top (daughter) board to come off. [<File:Upper_board_screw.jpg>](/File:Upper_board_screw.jpg "wikilink")

You should now be able to (gently) lift the top board (the board with the disk attached to it) straight up and off of the main (lower) board. Set the daughter board aside - you will not be doing anything else with it, but getting it out of the way makes it easier to get to the battery.

Now, we have to get to the battery itself. It's located under the same physical heatsink as the CPU, and not easily visible until the heatsink is removed: [<File:re_battery_hidden_by_heatsink.jpg>](/File:re_battery_hidden_by_heatsink.jpg "wikilink")

To do this we have to remove the heatsink. The heatsink is actually made up of two separately screwed-in components that have be removed in order. First, unscrew the two screws on the upper heatsink (with the fins on it): [<File:Heatsinks_assembled.jpg>](/File:Heatsinks_assembled.jpg "wikilink")

Now you will be able to unscrew the four screws for the lower (larger) heatsink component. Take these four screws out and then left off the lower heatsink. This will finally uncover the battery on the routing-engine board! [<File:heatsink_upper_removed.jpg>](/File:heatsink_upper_removed.jpg "wikilink")

Here is where the battery is mounted, clearly visible with the heatsink removed. (Coming soon: a picture with the battery actually installed when my replacement battery arrives..) [<File:Socket_without_battery.jpg>](/File:Socket_without_battery.jpg "wikilink")

Here is the actual battery itself: [<File:battery_top.jpg>](/File:battery_top.jpg "wikilink") [<File:battery_bottom.jpg>](/File:battery_bottom.jpg "wikilink")

It is a "TADIRAN TL-5186" battery. While not as common as a standard cr2032, the TL-5186 appears to be readily accessible from Digi-Key, Amazon, and various other sources.