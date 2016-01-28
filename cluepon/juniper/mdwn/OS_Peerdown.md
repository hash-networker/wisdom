---
title: OS Peerdown
permalink: /OS_Peerdown/
---

Information
-----------

This is my first op script to see what's possible with op scripts. One of my major annoyances was getting a quick view of peers not in 'established' state on a peering router and finding out which peer I have to bug to get it working again, so here's my 'peerdown' script.

Usage
-----

`user@router> op peerdown`
`peer IP                              peer ASN          downtime   peer description`
`==================================================================================`
`10.0.0.1                                65515           6:04:29   Some peer`
`10.0.0.2                                65516          13w5d11h   Another peer`

Bugs
----

I'm still in denial :)

ChangeLog
---------

1.0 - Initial Version
1.1 - Fix alignment