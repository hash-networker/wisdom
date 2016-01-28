---
title: Nhs.slax
permalink: /Nhs.slax/
---

`version 1.0;`
`ns junos = "`[`http://xml.juniper.net/junos/*/junos`](http://xml.juniper.net/junos/*/junos)`";`
`ns xnm = "`[`http://xml.juniper.net/xnm/1.1/xnm`](http://xml.juniper.net/xnm/1.1/xnm)`";`
`ns jcs = "`[`http://xml.juniper.net/junos/commit-scripts/1.0`](http://xml.juniper.net/junos/commit-scripts/1.0)`";`
`import "../import/junos.xsl";`
`template nhs ($family, $nexthop)`
`{   `
`    var $path = ../../../../policy-options;`
`    call jcs:emit-change($tag = 'transient-change', $dot = $path) {`
`        with $content = {`
`            `<policy-statement>` {`
`                `<name>` "NHS";`
`                `<term>` {`
`                    `<name>` $family;`
`                    `<from>` {`
`                        `<family>` $family;`
`                    }`
`                    `<then>` {`
`                        `<next-hop>` {`
`                            `

<address>
substring-before($nexthop, "/");

`                        }`
`                    }`
`                }`
`            }`
`        }`
`    }`
`}`
`match configuration {`
`    for-each (//interfaces/interface[name == "lo0"]/unit/family) {`
`        call nhs($family = "inet", $nexthop = inet/address/name);`
`        call nhs($family = "inet6", $nexthop = inet6/address/name);`
`    }`
`}`