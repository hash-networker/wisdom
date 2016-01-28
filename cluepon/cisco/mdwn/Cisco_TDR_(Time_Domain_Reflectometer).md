---
title: Cisco TDR (Time Domain Reflectometer)
permalink: /Cisco_TDR_(Time_Domain_Reflectometer)/
---

**Using Cisco's Integrated TDR**

The Catalyst 2960, 2970, 3560/3560-E, and 3750/3750-E switches have an integrated Time Domain Reflector (TDR), which is used to test cables associated with a port. TDR is supported only on 10/100/1000 and some 10/100 (Catalyst 2960) copper Ethernet ports. It is not supported on 10 GigabitEthernet or SFP module ports.

Helps with troubleshooting:

`   * Open wires`
`   * Cut wires`
`   * Broken wires`
`   * Shorted wires`

With the TDR functionallity you can find the length of individual wires.

Switch\# test cable-diagnostics tdr interface gigabitethernet0/2

TDR test started on interface Gi0/2

A TDR test can take a few seconds to run on an interface. Use "show cable-diagnostics tdr" to read the TDR results.

Switch\#show cable-diagnostics tdr interface gigabitEthernet 0/2

TDR test last run on: Dec 10 09:05:10

Interface Speed Local pair Cable length Remote channel Status

Gi0/1 0Mbps 1-2 102 +-2m Unknown Fault

3-6 100 +-2m Unknown Fault

4-5 102 +-2m Unknown Fault

7-8 102 +-2m Unknown Fault

The previous command is set to be deprecated sometime in the future and will be replaced by:

1.  show diagnostic result interface GigabitEthernet 0/1
