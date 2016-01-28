---
title: Using capture buffer to find interrupt switched traffic
permalink: /Using_capture_buffer_to_find_interrupt_switched_traffic/
---

The built-in capture buffer is very useful to look at the headers of traffic that is being interrupt switched so that you can diagnose the problem.

Note that this only works for interrupt switched traffic, i.e, that traffic that is causing the second % in your show proc cpu to be high. If your IP Input is high that is process switched traffic, and is found via show buffers input-interface <interface> packet

-   Turn on service internal.
-   Check to see that you have a PINNACLE ASIC in the supervisor.

`show plat cap buffer asic`

`Slot Cpu   Asic   Inst Ver PB Elam Active Filter`
`---- --- -------- ---- --- -- ---- ------ ------`
`1    0   HYPERION 0    6.0 Y`
`1    0   PINNACLE 0    4.0 Y`

-   Turn on the PINNACLE ASIC to capture traffic. Specify the slot of the active supervisor, and port 4 - direction out - priority lo. Ports 1 and 2 are the on-board GIGE ports, port 3 faces the SP, and port 4 faces the RP. Direction out means out from the ASIC towards the RP, whereas direction in would mean traffic from the RP. Priority lo refers to general IP traffic as opposed to high priority (routing protocols, etc).

`show plat cap buffer asic pinnacle slot 1 port 4 direction out priority lo`

-   Capture data for 5 seconds (or so).

`show plat cap buffer collect for 5`

-   Check the status of the capture session.

`show plat cap buffer status`

`Slot Cpu   Asic   Inst Ver PB Elam`
`---- --- -------- ---- --- -- ----`
`1    0   PINNACLE 0    4.0 Y`
`PB handling logical port 4, OUT direction, LOW queue`
`COS mapped on it: 0 1 2 3 4 5 6 7`
`CBL drops: 0`
`buffer size is 400 Kbytes [0x3200 lines of 32 bytes each]`
`it starts at 0xA600 [pointer is now 0xBF21]`
`filtering DISABLED`
`log is disabled. collection not intrusive. duration is 5 sec.`
`collection completed`
`280 UNFILTERED samples collected`

-   Then, look at the filtered samples.

`show plat cap buffer data filt`

-   Once you find a particular sample, look at it in more detail.

`show plat cap buffer data sample 100`

Decoding the output is up to you, but generally speaking you will be able to find out a particular pattern of either DMACs, SMACs, or something else that will help you to narrow it down.

[Cat 6000 high cpu](http://cisco.com/en/US/products/hw/switches/ps708/products_tech_note09186a00804916e0.shtml)