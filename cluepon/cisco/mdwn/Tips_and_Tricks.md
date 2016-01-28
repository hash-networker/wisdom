---
title: Tips and Tricks
permalink: /Tips_and_Tricks/
---

This section covers Tips and Tricks for Cisco routers. General tips & tricks will be listed here as well as platform specific options.

Protocols
=========

-   [HSRP](/HSRP "wikilink") - Host Standby Router Protocol

Diagnostics Tools
=================

[Cisco TDR (Time Domain Reflectometer)](/Cisco_TDR_(Time_Domain_Reflectometer) "wikilink") - Integrated cable tester

GSR Convergence Improvements
============================

`process-max-time 50`

This will lower the maximum time allocated for a process to run to 50ms rather than the default of 200ms.

`ip routing protocol purge interface`

This will allow an interface link state change to directly impact the RIB rather than waiting on routing protocol updates to the RIB to take place.

`ip cef table loadinfo force`

This will allow common prefixes on a LC-FIB to be associated with a pointer that permits rapid modification when the LC-FIB needs to be updated. This can save several seconds in convergence events on a network. Use caution with this command on E4/E4+ linecards as this increases LC memory usage.

     service internal

     ip cef linecard ipc service-timer 50

This will lower the timer (200ms default) used for transmitting XDR (essentially CEF writes) messages to a linecard.

Hidden Commands
===============

There are many hidden commands. The hidden command

    show parser dump

will allow you to reveal them by dumping the parser data (the unrolled parse tree) ,

    show parser dump exec

and

    show parser dump config

are the best way of doing this, along with

    show parser dump exec debug

which will write a file to your disk.

How to recover stuck telnet/ssh sessions
========================================

[How to recover stuck telnet/ssh sessions](/TCB_Recovery "wikilink")