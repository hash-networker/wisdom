---
title: OS Auto Tuning Prefix Limits
permalink: /OS_Auto_Tuning_Prefix_Limits/
---

Information
-----------

This Op Script integrates with [CS_Simplified_Prefix_Limits](/CS_Simplified_Prefix_Limits "wikilink") to automatically tune EBGP prefix-limits based on the number of prefixes currently being received over the active session. The script uses a weighted average of the current value and the new value (calculated as 130% of the current number of received prefixes + 400, rounded to the nearest 500) to slowly adjust the configured prefix-limit as a peer announces more or fewer prefixes.

Usage
-----

`user@router> op tunepfxlimits`
`Updated Prefix Limit - Neighbor: 1.2.3.20 Old: 2500 New: 2625`
`Updated Prefix Limit - Neighbor: 1.2.3.79 Old: 10000 New: 8500`
`Updated Prefix Limit - Neighbor: 1.2.3.113 Old: 2000 New: 2125`
`Updated Prefix Limit - Neighbor: 1.2.3.64 Old: 500 New: 625`
`Updated Prefix Limit - Neighbor: 1.2.3.21 Old: 500 New: 625`
`Updated Prefix Limit - Neighbor: 1.2.3.82 Old: 3000 New: 2500`
`Updated Prefix Limit - Neighbor: 1.2.3.51 Old: 2000 New: 1750`
`Updated Prefix Limit - Neighbor: 1.2.3.5 Old: 500 New: 625`
`Updated Prefix Limit - Neighbor: 1.2.3.16 Old: 500 New: 625`
`Updated Prefix Limit - Neighbor: 1.2.3.88 Old: 10 New: 133`
`Updated Prefix Limit - Neighbor: 1.2.3.54 Old: 2000 New: 1750`
`Updated Prefix Limit - Neighbor: 1.2.3.123 Old: 800 New: 975`
`Updated Prefix Limit - Neighbor: 1.2.3.39 Old: 200 New: 275`
`Updated Prefix Limit - Neighbor: 1.2.3.18 Old: 5000 New: 4250`
`Updated Prefix Limit - Neighbor: 1.2.3.30 Old: 2000 New: 1875`
`Updated Prefix Limit - Neighbor: 1.2.3.69 Old: 200 New: 275`
`Updated Prefix Limit - Neighbor: 1.2.3.56 Old: 500 New: 625`
`Updated Prefix Limit - Neighbor: 1.2.3.40 Old: 20 New: 140`
`Updated Prefix Limit - Neighbor: 1.2.3.46 Old: 3000 New: 3125`
`Updated Prefix Limit - Neighbor: 1.2.3.22 Old: 500 New: 625`
`Updated Prefix Limit - Neighbor: 1.2.3.58 Old: 300 New: 475`
`Updated Prefix Limit - Neighbor: 1.2.3.2 Old: 100 New: 200`
`Updated Prefix Limit - Neighbor: 1.2.3.84 Old: 20 New: 140`
`user@router> configure `
`Entering configuration mode`
`The configuration has been changed but not committed`
`[edit]`
`user@router# commit and-quit`

Bugs
----

As of JUNOS 8.3 proper configuration locking and committing from an Op Script is still not functional. This script can still be called from an Event script, but care should be taken in the event that another user is making configuration changes when such a script runs. Hopefully this will be resolved in the near future.

This script currently only works for inet-unicast families. It can easily be extended to support all AFI/SAFI's, but until the issues above are resolved this script is currently recommended only as a proof of concept or for manual execution.

As of JUNOS 9.2, committing from the Op Script should now be possible.

ChangeLog
---------

1.0 - Initial Version