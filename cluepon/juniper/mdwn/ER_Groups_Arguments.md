---
title: ER Groups Arguments
permalink: /ER_Groups_Arguments/
---

Problem
-------

Many configuration changes are repeated save for a few bits of information. The 'Groups' command works well for reusing configuration that is repeated, but it is static and must be identical at each place it is applied.

`groups {`
`    copper {`
`        description "A shiny new 100baseT interface";`
`        speed 100m;`
`        link-mode full-duplex;`
`    }`
`}`
`interfaces {`
`    ge-1/1/9 {`
`        apply-groups copper;`
`        unit 0 {`
`            family inet {`
`                address 10.10.10.10/24;`
`            }`
`        }`
`   }`
`}`

Besides having to apply many lines of config to make a single routine addition, this method also makes it easy for different users to apply the config in their own 'personal' way, creating inconsistencies.

Solution
--------

Allow the 'groups' command to call 'arguments' that can be used in the group definition by specifying an argument variable.

For example:

`groups {`
`    copper {`
`        description $1;`
`        speed 100m;`
`        link-mode full-duplex;`
`        unit 0 {`
`            family inet {`
`                address $2;`
`            }`
`        }`
`    }`
`}`
`interfaces {`
`    ge-1/1/9 {`
`        apply-groups copper "Port 1, Bay 2, Rack 3, Room 4" 10.10.10.10/24;`
`   }`
`}`
`interfaces {`
`    ge-1/1/3 {`
`        apply-groups copper "Port 15, Bay 2, Rack 3, Room 4" 20.20.20.20/24;`
`   }`
`}`

The ‘apply-group copper’ line above has two arguments, the first a description, and the second an IP address. They get placed into the group definition where $1 and $2 are, respectively. Due to internal use, $1 may not be a good variable name. Any alternative may be used, i.e. %1, &1, etc.

Preferably syntax checking would look to make sure the number of arguments on the apply-groups command matched the number in the 'groups' definition, and result in a \*warning\* if they did not match and return a null string for undefined arguments, and ignore excess arguments. This change would not break existing use of the 'groups' and 'apply-groups' commands.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")