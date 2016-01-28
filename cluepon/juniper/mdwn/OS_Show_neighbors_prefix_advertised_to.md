---
title: OS Show neighbors prefix advertised to
permalink: /OS_Show_neighbors_prefix_advertised_to/
---

Information
-----------

A quick and dirty replication of Cisco's "show ip bgp" functionality which shows which BGP neighbors a route is advertised to.

Usage
-----

`user@router> op advertised prefix 1.2.3.0/24`
`Prefix 1.2.3.0/24 is advertised to the following BGP neighbors:`
` * 2.3.4.5 (Example BGP Peer Description)`
` * 2.3.4.6 (Example BGP Peer Description)`
` * 2.3.4.7 (Example BGP Peer Description)`

Bugs
----

Should support stuff like more/less specifics, exact match only for now. A little slow too...

ChangeLog
---------

1.0 - Initial Version - Inspired by 'grigs to silence whiney ops people.