---
title: Pfxlimit.slax
permalink: /Pfxlimit.slax/
---

`version 1.0;`
`ns junos = "`[`http://xml.juniper.net/junos/*/junos`](http://xml.juniper.net/junos/*/junos)`";`
`ns xnm = "`[`http://xml.juniper.net/xnm/1.1/xnm`](http://xml.juniper.net/xnm/1.1/xnm)`";`
`ns jcs = "`[`http://xml.juniper.net/junos/commit-scripts/1.0`](http://xml.juniper.net/junos/commit-scripts/1.0)`";`
`import "../import/junos.xsl";`
`template pfxlimit($neighbor, $afi, $safi, $limit)`
`{`
`    call jcs:emit-change($tag = 'transient-change', $dot = ../..) {`
`        with $content = {`
`            `<xsl:element name=$afi>` {`
`                `<xsl:element name=$safi>` {`
`                    `<prefix-limit>` {`
`                        `<maximum>` $limit;`
`                        `<teardown>` {`
`                        `<limit-threshold>` 90;`
`                            `<idle-timeout>` {`
`                                `<timeout>` 5;`
`                            }`
`                        }`
`                    }`
`                }`
`            }`
`        }`
`    }`
`}`
`match configuration {`
`    /* Parse BGP neighbors with a prefix-limit macro configured */`
`    for-each (protocols/bgp/group/neighbor/apply-macro[name = 'prefix-limit']) {`
`        var $family = jcs:first-of(../family, ../../family, ../../../family);`
`        var $neighbor = ../name;`
`        /* Preserve optional higher level AFI/SAFI selections */`
`        call jcs:emit-change($tag = 'transient-change', $dot = ..) {`
`            with $content = {`
`                copy-of $family;`
`            }`
`        }`
`        for-each (data) {`
`            var $limit = value;`
`            if (name == 'all') {`
`                call pfxlimit($neighbor, $afi = "inet", $safi = "any", $limit);`
`                call pfxlimit($neighbor, $afi = "inet6", $safi = "any", $limit);`
`            } else if (name == 'inet') {`
`                call pfxlimit($neighbor, $afi = "inet", $safi = "any", $limit);`
`            } else if (name == 'inet-unicast') {`
`                call pfxlimit($neighbor, $afi = "inet", $safi = "unicast", $limit);`
`            } else if (name == 'inet-multicast') {`
`                call pfxlimit($neighbor, $afi = "inet", $safi = "multicast", $limit);`
`            } else if (name == 'inet6') {`
`                call pfxlimit($neighbor, $afi = "inet6", $safi = "any", $limit);`
`            } else if (name == 'inet6-unicast') {`
`                call pfxlimit($neighbor, $afi = "inet6", $safi = "unicast", $limit);`
`            } else if (name == 'inet6-multicast') {`
`                call pfxlimit($neighbor, $afi = "inet6", $safi = "multicast", $limit);`
`            }`
`        }`
`    }`
`}`