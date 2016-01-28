---
title: ER BGP TTL Security support enhancements
permalink: /ER_BGP_TTL_Security_support_enhancements/
---

Problem
-------

Currently, Juniper has no direct configuration support for the Generalized TTL Security Mechanism (GTSM). In order to implement GTSM, a user must configure something like:

`policy-options {`
`    prefix-list GTSM-Neighbors {`
`        apply-path "protocols bgp group <*> neighbor `<gtsm-*>`";`
`        -or-`
`        manual configuration of neighbors;`
`    }`
`}`
`protocols {`
`    bgp {`
`        group gtsm-specific-example {`
`            multihop {`
`               ttl 255;`
`            }`
`            neighbor x.x.x.x {`
`                ...`
`            }`
`        }`
`    }`
`}`
`firewall {`
`    filter control-plane-filter {`
`        ...`
`        term gtsm {`
`            from {`
`                source-prefix-list {`
`                    gtsm-neighbors;`
`                }`
`                protocol tcp;`
`                port 179;`
`                ttl-except 254;`
`            }`
`            then {`
`                discard;`
`            }`
`        }`
`        ...`
`    }`
`}`
`interface {`
`    lo0 {`
`        unit 0 {`
`            family inet {`
`                filter {`
`                    input control-plane-filter;`
`                }`
`            }`
`        }`
`    }`
`}`

In addition to being very complex and inconvenient to configure, this requires T-series and newer M-series routers to provide any level of protection with TTL matching in firewall.

Solution
--------

Provide a simple configuration flag at the neighbor level which performs all of the actions necessary to support GTSM, similar to the configuration method used by Cisco.

`protocols {`
`    bgp {`
`        group ordinary-group {`
`            neighbor x.x.x.x {`
`                ...`
`                ttl-security;`
`            }`
`        }`
`    }`
`}`

This can also be used to provide a basic level of filtering at the Routing Engine level, even if the hardware is older and can not support TTL matching in firewall. This would eliminate the need to configure inter-provider MD5 passwords in order to achieve basic protection of the TCP session in many circumstances.

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")