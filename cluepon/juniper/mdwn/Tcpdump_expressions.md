---
title: Tcpdump expressions
permalink: /Tcpdump_expressions/
---

Problem
-------

Juniper PICs strip the link-layer headers before passing packets to the PFE. Unfortunately, the lack of link-layer headers breaks some versions of tcpdump's pcap expression matching functions.

Not working:

`root@router% tcpdump -h`
`Version 3.5`

Working:

`JunOS 7.2R1.7`
`root@router% tcpdump -h`
`Version 4.0-PRE-CVS`

Not certain exactly what version this is fixed in, something recent.

Solution
--------

A quick fix for this on older versions is to write the packet captures in tcpdump binary format to stdout, and pipe it to another tcpdump invocation which reads the packet captures from stdin. This resets the missing link-layer headers and makes expressions work again.

Example
-------

`user@router> start shell tcsh `
`> su -`
`Password:`
`root@router% tcpdump -w - | tcpdump -n -r - port 22 and host 10.0.0.1`
`Listening on fxp0, capture size 96 bytes`