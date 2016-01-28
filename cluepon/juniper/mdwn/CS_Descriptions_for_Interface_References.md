---
title: CS Descriptions for Interface References
permalink: /CS_Descriptions_for_Interface_References/
---

Information
-----------

This script searches for references to physical and logical interfaces under the "protocols" hierarchy and automatically copies the descriptions from the interface as "annotate" style comments.

Usage
-----

`interfaces {`
`    xe-1/0/0 {`
`        description "This is an example";`
`        unit 666 {`
`            ....`
`        }`
`        unit 1234 {`
`            description "Yet another example";`
`            ....`
`        }`
`    }`
`}`
`protocols {`
`    isis {`
`        interface xe-1/0/0.666;`
`        interface xe-1/0/0.1234;`
`    }`
`    mpls {`
`        interface xe-1/0/0.666;`
`        interface xe-1/0/0.1234;`
`    }`
`}`

Will automatically create:

`protocols {`
`    isis {`
`        /* This is an example */`
`        interface xe-1/0/0.666;`
`        /* Yet another example */`
`        interface xe-1/0/0.1234;`
`    }`
`    mpls {`
`        /* This is an example */`
`        interface xe-1/0/0.666;`
`        /* Yet another example */`
`        interface xe-1/0/0.1234;`
`    }`
`}`

Bugs
----

Currently ignores references outside the scope of "protocols", such as "class-of-service". This version uses // to search for all references, but I don't currently have a way to exclude trying to set comments under the "interfaces" hierarchy where it copies the comments from. A quick workaround for this was to restrict it to the protocols hierarchy, but I'd really like to find a way to exclude the unwanted section rather than only look at part of the config.

ChangeLog
---------

1.0 - Initial Version