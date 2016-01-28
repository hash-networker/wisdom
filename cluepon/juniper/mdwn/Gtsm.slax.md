---
title: Gtsm.slax
permalink: /Gtsm.slax/
---

`version 1.0;`
`ns junos = "`[`http://xml.juniper.net/junos/*/junos`](http://xml.juniper.net/junos/*/junos)`";`
`ns xnm = "`[`http://xml.juniper.net/xnm/1.1/xnm`](http://xml.juniper.net/xnm/1.1/xnm)`";`
`ns jcs = "`[`http://xml.juniper.net/junos/commit-scripts/1.0`](http://xml.juniper.net/junos/commit-scripts/1.0)`";`
`import "../import/junos.xsl";`
`match configuration {`
`    /* Find all configured BGP Sessions with the ttl-security macro */`
`    var $ttl = protocols/bgp/group/neighbor/apply-macro[name = 'ttl-security'];`
`    /* Configure multihop with TTL 255 for the matched sessions */`
`    for-each ($ttl) {`
`        call jcs:emit-change($tag = 'transient-change', $dot = ..) {`
`            with $content = {`
`                `<multihop>` {`
`                    `<ttl>` 255;`
`                }`
`            }`
`        }`
`    }`
`    /* Build IPv4 and IPv6 prefix-lists of the GTSM neighbor addresses */`
`    `<transient-change>` {`
`        `<policy-options>` {`
`            `<prefix-list replace="replace">` {`
`                `<name>` "GTSM-IPV4-NEIGHBORS";`
`                for-each ($ttl) {`
`                    if (contains(../name, ".")) {`
`                        `<prefix-list-item>` {`
`                            `<name>` ../name;`
`                        }`
`                    }`
`                }`
`            }`
`            `<prefix-list replace="replace">` {`
`                `<name>` "GTSM-IPV6-NEIGHBORS";`
`                for-each ($ttl) {`
`                    if (contains(../name, ":")) {`
`                        `<prefix-list-item>` {`
`                            `<name>` ../name;`
`                        }`
`                    }`
`                }`
`            }`
`        }`
`    } `
`}`