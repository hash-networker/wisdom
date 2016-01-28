---
title: ER Show arp enhancement
permalink: /ER_Show_arp_enhancement/
---

Problem
-------

Currently, [JunOS](/JunOS "wikilink") does not display the same amount of detail under the "show arp" command that other router vendors do.

Solution
--------

Display the age of the dynamically learned ARP cache entries, and entries which are being queried but which are still incomplete/unresolved.

For example:

`user@router> show arp no-resolve    `
`MAC Address       Address         Interface     Flags   Age`
`00:12:43:58:cd:80 1.2.3.4         ge-0/0/0.0    none    174s`
`00:90:69:3c:7b:13 1.2.3.5         ge-0/0/0.0    none    400s`
`00:0d:61:76:e8:90 1.2.3.6         ge-0/0/0.0    none    31s`
`00:0d:61:9d:a2:8f 1.2.3.7         Incomplete    none `

Status
------

show arp expiration-time added in JUNOS 8.1.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")