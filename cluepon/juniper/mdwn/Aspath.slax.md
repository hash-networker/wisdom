---
title: Aspath.slax
permalink: /Aspath.slax/
---

`version 1.0;`
`ns junos = "`[`http://xml.juniper.net/junos/*/junos`](http://xml.juniper.net/junos/*/junos)`";`
`ns xnm = "`[`http://xml.juniper.net/xnm/1.1/xnm`](http://xml.juniper.net/xnm/1.1/xnm)`";`
`ns jcs = "`[`http://xml.juniper.net/junos/commit-scripts/1.0`](http://xml.juniper.net/junos/commit-scripts/1.0)`";`
`import "../import/junos.xsl";`
`match configuration {`
`    for-each (//from/as-path[jcs:regex("^AS([0-9]+)$", .)]) {`
`        `<transient-change>` {`
`            `<policy-options>` {`
`                `<as-path replace="replace">` {`
`                    `<name>` .;`
`                    `<path>` ".*" _ substring-after(., "AS") _ ".*";`
`                }`
`            }`
`        }`
`    }`
`}`