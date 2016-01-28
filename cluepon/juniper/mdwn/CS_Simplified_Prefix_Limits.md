---
title: CS Simplified Prefix Limits
permalink: /CS_Simplified_Prefix_Limits/
---

Information
-----------

This script provides a simplified interface to creating and managing prefix-limits on BGP neighbors by separating the prefix-limit configuration from the AFI/SAFI configuration.

By providing a separate configuration for prefix-limits, configure BGP "family" settings can be inherited from higher-level groups without requiring a complete restating of the family settings for every BGP neighbor. This fixes an area of common misconfiguration where a user changing a prefix-limit accidentally causes a BGP session to bounce because the family configurations have changed. This script also significantly reduces the size and complexity of the BGP configuration in the event that you have a standardized teardown time and warning threshold, since they do not need to be restated every time.

Usage
-----

`protocols {`
`    bgp {`
`        group example {`
`           family {`
`               inet {`
`                   unicast;`
`               }`
`               inet6 {`
`                   any;`
`               }`
`           }`
`           neighbor 1.2.3.4 {`
`               peer-as 1234;`
`               apply-macro prefix-limit {`
`                   all 5000;`
`                   inet-unicast 750;`
`               }`
`           }`
`           neighbor 2.3.4.5 {`
`               peer-as 2345;`
`               apply-macro prefix-limit {`
`                   inet-unicast 250;`
`               }`
`           }`
`        }`
`    }`
`}`

Will automatically create:

`protocols {`
`    bgp {`
`        group example {`
`           family {`
`               inet {`
`                   unicast;`
`               }`
`               inet6 {`
`                   any;`
`               }`
`           }`
`           neighbor 1.2.3.4 {`
`               peer-as 1234;`
`               family {`
`                   inet {`
`                       unicast {`
`                           prefix-limit {`
`                               maximum 750;`
`                               teardown 90 idle-timeout 5;`
`                           }`
`                       }`
`                   }`
`                   inet6 {`
`                       any {`
`                           prefix-limit {`
`                               maximum 5000;`
`                               teardown 90 idle-timeout 5;`
`                           }`
`                       }`
`                   }`
`               }`
`           }`
`           neighbor 2.3.4.5 {`
`               peer-as 2345;`
`               family {`
`                   inet {`
`                       unicast {`
`                           prefix-limit {`
`                               maximum 500;`
`                               teardown 90 idle-timeout 5;`
`                           }`
`                       }`
`                   }`
`                   inet6 {`
`                       any;`
`                   }`
`               }`
`            }`
`        }`
`    }`
`}`

Bugs
----

There is currently no way to configure the teardown threshold and restart timer, the values are hard-coded into the script. Future versions may want to provide a way to create "configurations" for these values, and support inheriting them from higher levels.

Currently this script only functions with macros configured at the neighbor level, not the group or top-level bgp level.

The configured AFI/SAFI may still be manipulated by configuring a prefix-limit for an AFI/SAFI which does not exist in the family configuration. No attempt is made in this version to prevent adding of new family configurations (which may cause BGP session resets), only to copy the the existing values from higher level configurations and prevent removing family configurations by defining new per-neighbor prefix-limits. Special care should be paid when mixing "inet" (meaning inet/any) with "inet-unicast/inet-multicast". This may be a reasonable area to enhance in the future, but for many networks the existing script will be sufficient to simplify their configurations.

ChangeLog
---------

1.0 - Initial Version