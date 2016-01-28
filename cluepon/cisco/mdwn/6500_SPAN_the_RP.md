---
title: 6500 SPAN the RP
permalink: /6500_SPAN_the_RP/
---

6500s (with appropriate software revisions) can SPAN (port mirror) the interface facing the CPU, allowing you to view all the CPU-punted traffic

Monitoring under SXI
--------------------

It's really easy under SXI

1) Create the monitor session of choice - the following example uses ERSPAN

`monitor session 1 type erspan-source`
` description foobar`
` source cpu [rp|sp]`
` destination`
`  erspan-id ANUMBER`
`  ip address the.receiver.ip.addr`
`  ip ttl 64`
`  origin ip address the.loopback.ip.addr`
`  ! optional`
`  vrf WHATEVER`
` no shut`

2) Use something to receive the traffic - see below for ERSPAN

Monitoring under SXF
--------------------

At the time of writing (march 2007, IOS 12.2SXF7) you need a dummy span session and to talk to the SP IOS. Proceed as follows:

1) Determine which slot your active (or only, if you have just one) supervisor is in - "show mod" and "show red" should get this

2) Create the "dummy" SPAN session. The following example is an ERSPAN (SPAN over GRE), but ordinary SPAN and RSPAN can be used. GiX/Y is any port which is down/not in use:

`monitor session 1 type erspan-source`
` description foobar`
` source interface GiX/Y`
` destination`
`  erspan-id ANUMBER`
`  ip address the.receiver.ip.addr`
`  ip ttl 64`
`  origin ip address the.loopback.ip.addr`
`  ! optional`
`  vrf WHATEVER`

3) attach to the active sup SP IOS - X is the slot you determined in step 1

`attach X`

4) add the RP port to the session you just created

`test monitor add 1 rp-inband both`

5) And/or add the SP port (layer 2 traffic)

`test monitor add 1 sp-inband both`

5) exit back to the main IOS

`exit`

6) enable the span session

`conf t`
`mon sess 1`
`no shut`

7) you should see traffic flowing

ERSPAN to a non-cisco
---------------------

If you're using ERSPAN, you can use [gulp](http://staff.washington.edu/corey/gulp/) to receive and view the traffic on something that isn't another Cisco