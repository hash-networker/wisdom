---
title: CS MPLS AutoMesh
permalink: /CS_MPLS_AutoMesh/
---

Information
-----------

This script automatically builds a complete mesh of label-switched-paths using a non-transient configuration from the current router to all other routers, based on a simple list of MPLS-speaking routers which can be uploaded to every device through Netconf or other management systems.

Usage
-----

`system {`
`    host-name rtr1.example1;`
`}`
`protocols {`
`    mpls {`
`        apply-macro automesh {`
`           rtr1.example1 1.2.3.1;`
`           rtr2.example1 1.2.3.2;`
`           rtr1.example2 1.2.4.1;`
`           rtr2.example2 1.2.4.2;`
`           rtr1.example3 1.2.5.1;`
`           rtr2.example3 1.2.5.2;`
`           rtr1.example4 1.2.6.1;`
`           rtr2.example4 1.2.6.2;`
`        }`
`    }`
`}`

Will automatically create the full mesh:

`protocols {`
`    mpls {`
`        label-switched-path RTR1.EXAMPLE1-RTR2.EXAMPLE1 {`
`            to 1.2.3.2;`
`        }`
`        label-switched-path RTR1.EXAMPLE1-RTR1.EXAMPLE2 {`
`            to 1.2.4.1;`
`        }`
`        label-switched-path RTR1.EXAMPLE1-RTR2.EXAMPLE2 {`
`            to 1.2.3.2;`
`        }`
`        label-switched-path RTR1.EXAMPLE1-RTR1.EXAMPLE3 {`
`            to 1.2.4.1;`
`        }`
`        label-switched-path RTR1.EXAMPLE1-RTR2.EXAMPLE3 {`
`            to 1.2.3.2;`
`        }`
`        label-switched-path RTR1.EXAMPLE1-RTR1.EXAMPLE4 {`
`            to 1.2.4.1;`
`        }`
`        label-switched-path RTR1.EXAMPLE1-RTR2.EXAMPLE4 {`
`            to 1.2.3.2;`
`        }`
`    }`
`}`

Bugs
----

The script determines which device in the list is the "current" router by comparing the device name and local hostname. If your hostname is not an exact match for the device name listed in the automesh macro, the script won't work, so you may also need to use [CS_Dual_RE_Hostname_Manager](/CS_Dual_RE_Hostname_Manager "wikilink") to manage your hostname for dual-RE routers.

I also have a personal preference for lsp names in the form of "RTRA-RTRB", in all capital letters. Currently the Juniper scripts are based on XSLT 1.1, which lacks an uppercase or lowercase function, so I've implemented the capitalization using a ghetto little hack which you can see in the code. There are indications that future JUNOS versions will support XSLT 2.0 functions as well.

The current implementation will overwrite any changes to make to the "to" destination address for a label-switch-path using a script-created name, but will not stop you from adding other configurations inside the LSP or from deactivating the LSP if you would like. An idea for a future version is to include a warning if there is a configured LSP which matches the auto-generated format but which does not exist in the auto-mesh configuration. Currently this script only creates LSPs, it makes no attempt to remove old ones.

A more complex network making use of other Commit Scripts may want to have a single configured automesh list located out of the MPLS hierarchy, which could be used to build other configurations such as IBGP neighbors and firewall filters. Changes to this script to support such a configuration are left as an exercise for the reader.

ChangeLog
---------

1.0 - Initial Version