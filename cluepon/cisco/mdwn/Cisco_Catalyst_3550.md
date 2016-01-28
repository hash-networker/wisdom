---
title: Cisco Catalyst 3550
permalink: /Cisco_Catalyst_3550/
---

[Category:Switch](/Category:Switch "wikilink")

[thumb|right|Cisco 3550 Series Switch Routers](/Image:c3550.jpg "wikilink")

Introduction
------------

The Cisco 3550 is a series of L2/L3 switch routers designed for use in corporate network environments. The 3550 series switches include IP routing and L2/L3 QoS functionality.

They have been EoS'ed in May 2006 and their featureset has largely been replaced by the Cisco 3560.

The Cisco 3550 uses TCAM to implement the L2/L3 features. This allows them to switch and route at a rather high rate but does put limits on various resources (MAC entries, routing table entries, ACL entries, etc.) BGPv4 is available but the 3550 TCAM falls far short of being able to hold a complete Internet routing table. Even with the limited featureset the 3550 can still route over a gigabit of traffic making it attractive to small-scale hosting and internet access providers.

Limitations
-----------

-   TCAM memory is limited (depending on 3550 model); limiting MAC, QoS, routing and ACL entries - be sure you keep an eye on TCAM usage as the failure modes may not be very pretty
-   The hardware-forwarding architecture is very fast but limits various features such as class-map matching options, policing vs shaping, various traffic interception/redirection options such as WCCPv2.
-   The core memory and flash isn't upgradable.
-   No IPv6 support.

Tips and Tricks
---------------

-   QoS and flow-control both enabled result in an abundance of PAUSE frames being sent and many RX'ed frames being dropped topping the throughput of gigabit ports to under 300mbit/sec. Disabling QoS or disabling flow-control on the gigabit interfaces will rectify the issue.
-   Be sure to check the SDM template in use and select the one which best suits your environment. The TCAM allocations are fixed in each template (in contrast to some of the earlier switches). Note that it'll require a reboot.
