---
title: ER BGP overload
permalink: /ER_BGP_overload/
---

When the router boots up (or is recovering from RE failure), the BGP sessions seem to come up in a very varying latency. For example, during one software upgrade, the first session came up at 20:25:33.750 and the last 20:27:09.167; that's over 90 seconds (we have roughly 20-30 peers in this box, we get the full routing table from 2). In addition, extra latency is caused by the transfer of routing tables. So, in the worst case, we're talking about 2 minutes of partial information.

We must make the setup of BGP sessions more deterministic, because partial information when the router does not have full routing information leads to loops and reduced availability.

One way to make this more deterministic could be to implement an "overload" and "overload timeout" setting for global/peer-group/peer configuration, in a slightly similar fashion as it exists for IS-IS and OSPF. It could have the following characteristics:

-   when set, the router establishes BGP sessions, receives the routes, and runs the decision process as normal, but does not advertise _any_ prefix yet (either to a BGP peer -- in theory, one should restrict redistribution as well, but that's not an issue for us).

<!-- -->

-   when the "overload over" condition is met, start advertising all the prefixes it would normally advertise. (You could sum this up as an empty export policy until "overload over" condition is met.)

<!-- -->

-   "overload over" condition could be one of the following, for example:

`1) XXX seconds (the overload timeout) has passed since rpd was restarted`
`   (like OSPF/IS-IS overload timeout)`
`                                                                                                                       `
`2) YY seconds has passed since the last of the configured BGP sessions`
`   came up.`