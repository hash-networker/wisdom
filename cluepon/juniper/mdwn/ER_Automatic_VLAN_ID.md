---
title: ER Automatic VLAN ID
permalink: /ER_Automatic_VLAN_ID/
---

Problem
-------

Moving vlan-tagged Ethernet units is a two-step process:

`interface ge-0/0/0 {`
`    vlan-tagging;`
`    unit 101 {`
`        vlan-id 101;`
`        ...`
`    }`
`    ...`
`}`
`[edit interfaces ge-0/0/0]`
`user@router# rename unit 101 to unit 102 `
`[edit interfaces ge-0/0/0]`
`user@router# set unit 102 vlan-id 102 `

This may be unnecessary if you follow a consistent mapping of unit IDs and vlan IDs.

Solution
--------

Add a new option to the "vlan-id" directive, allowing it to automatically map to the unit ID.

For example:

`interface ge-0/0/0 {`
`    vlan-tagging;`
`    unit 101 {`
`        vlan-id unit-id;`
`        ...`
`    }`
`    ...`
`}`

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")