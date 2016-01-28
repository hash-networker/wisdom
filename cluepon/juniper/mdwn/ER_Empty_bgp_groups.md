---
title: ER Empty bgp groups
permalink: /ER_Empty_bgp_groups/
---

Problem
-------

Currently, [JunOS](/JunOS "wikilink") does not allow a BGP group to exist with no neighbors defined under it. This can be useful for catching simple misconfigurations where a group is created with no neighbors under it, but it is better implemented as a warning rather than a hard error. Unfortunately, users often pre-configure their router groups for certain roles, which may not immediately be used. Also, users who go to deactivate the last neighbor in a group find that the configuration will not commit without deactivating the entire group. The users must then remember to re-activate the group later.

For example:

`protocols bgp {`
`    group example {`
`        neighbor 1.2.3.4 {`
`            description "Example Neighbor";`
`            remote-as 1234;`
`        }`
`    }`
`}`
`[edit protocols bgp group example]`
`user@router# deactivate neighbor 1.2.3.4 `
`user@router# commit check `
`[edit protocols]`
`  'bgp'`
`    BGP group example must have either configured peers or an allow list`
`error: configuration check-out failed`

Solution
--------

Allow BGP groups to exist with no neighbors defined. It may be reasonable to generate a warning, and to optimize away the group (automatically "deactivating" it internally) upon commit, but it should not be an error to have a group with no active neighbors.

Another potential solution is to make this (and possibly other similar errors) configurable, to either generate hard errors, generate warnings, or ignore them completely. This way, routers could ship with hard errors enabled by default to help users catch errors, but could be easily ignored by power users.

Status
------

This feature is available in JunOS 7.4.

The release notes of JunOS 10.1 mentions the following:

"Removal of BGP warning message — If a BGP group is created without any defined peers, the warning message no longer appears when the configuration is committed."