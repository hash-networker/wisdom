---
title: Show bgp targets
permalink: /Show_bgp_targets/
---

Displays route targets active on the router.

Examples
--------

    user@juniper> show bgp targets
    Target communities:
    100:1 :
      Ribgroups:
        vpn-unicast target:100:1    Family: l3vpn   Refcount: 1
            Export RIB: l3vpn.0
            Import RIB: bgp.l3vpn.0 Secondary: VPN-A.inet.0
    100:2 :
      Ribgroups:
        vpn-unicast target:100:2    Family: l3vpn   Refcount: 1
            Export RIB: l3vpn.0
            Import RIB: bgp.l3vpn.0 Secondary: VPN-B.inet.0