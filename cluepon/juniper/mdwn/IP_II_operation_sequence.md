---
title: IP II operation sequence
permalink: /IP_II_operation_sequence/
---

The IP II (a.k.a CF-chip) performs multiple operations like filtering, lookup, RPF. The graph below shows the order of task execution.

if-x ---&gt; if-x-input-filter -- \[ scu / rpf \] -- \[ftf\] -- dst lookup -- if-y-output-filter -- if-y encaps --- \[out\]

"FTF" is Forwarding Table Filtering. "dst lookup" in addition to forwarding decision is also a place where DCU for a given packet is resolved.

Understanding this sequence can solve many problems during implementation of PBR, Packet Filters, etc.

Source: j-nsp thread "Filter created from bgp route tag". Post of Pedro Roque Marques \[roque@juniper.net\]