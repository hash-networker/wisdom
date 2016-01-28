---
title: ER Enhance monitor commands
permalink: /ER_Enhance_monitor_commands/
---

Problem
-------

The CLI-interactive "monitor" commands are among the most powerful and useful features offered by JUNOS. Many users already take advantage of the "monitor interface" and "monitor interface traffic" commands, but it has not been enhanced since the earliest days of JUNOS. There are many additional features which can be implemented, and many new applications for which monitor commands would be appropriate.

Monitor Firewall
----------------

-   Implement a "monitor firewall" command which interactively displays both firewall counters and firewall logs. In addition to simply refreshing the display with new counter updates, a "monitor firewall counters" can display the rate at which counters are increasing in bytes/pkts per second.

Monitor Interface Traffic
-------------------------

-   Add support for SI units (Kilo Mega Giga etc) to make monitor results more easily readable.

Generic Monitor Command
-----------------------

-   Implement a generic "monitor" command which repeats any arbitrary CLI command using the monitor curses interface.

Other enhancements
------------------

-   Add a command to clear/zero the counters inside an interactive "monitor".

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")