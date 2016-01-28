---
title: Iface static.slax
permalink: /Iface_static.slax/
---

`version 1.0;`
`ns junos = "`[`http://xml.juniper.net/junos/*/junos`](http://xml.juniper.net/junos/*/junos)`";`
`ns xnm = "`[`http://xml.juniper.net/xnm/1.1/xnm`](http://xml.juniper.net/xnm/1.1/xnm)`";`
`ns jcs = "`[`http://xml.juniper.net/junos/commit-scripts/1.0`](http://xml.juniper.net/junos/commit-scripts/1.0)`";`
`import "../import/junos.xsl";`
`template static($prefix, $nexthop, $rib, $iface)`
`{`
`    `<transient-change>` {`
`        `<routing-options>` {`
`            `<rib>` {`
`                `<name>` $rib;`
`                `<static>` {`
`                    `<junos:comment replace="replace">` {`
`                        expr "Inferface-Generated Static Route: " _ $iface;`
`                    }`
`                    `<route>` {`
`                        `<name>` $prefix;`
`                        `<next-hop>` $nexthop;`
`                    }`
`                }`
`            }`
`        }`
`    }`
`}`
`match configuration {`
`    var $top = .;`
`    /* Scan the interface configurations for static route macros */`
`    for-each (interfaces/interface/unit/family/*/apply-macro[name = "static"]) {`
`        var $iface = concat(../../../../name, ".", ../../../name);`
`        var $rib = name(..) _ ".0";`
`        /* Walk through the static route macro */`
`        for-each (data) {`
`            var $route = name;`
`            var $nexthop = value;`
`            var $pfxlist = $top/policy-options/prefix-list[name == $route];`
`            if ($pfxlist) {`
`                /* If the route is a prefix-list, expand it */`
`                for-each ($pfxlist/prefix-list-item) {`
`                    call static($prefix = name, $nexthop, $rib, $iface);`
`                }`
`            } else {`
`                /* If not, it must be a single route */`
`                call static($prefix = $route, $nexthop, $rib, $iface);`
`            }`
`        }`
`    }`
`}`