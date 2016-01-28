---
title: Hostname.slax
permalink: /Hostname.slax/
---

`version 1.0;`
`ns junos = "`[`http://xml.juniper.net/junos/*/junos`](http://xml.juniper.net/junos/*/junos)`";`
`ns xnm = "`[`http://xml.juniper.net/xnm/1.1/xnm`](http://xml.juniper.net/xnm/1.1/xnm)`";`
`ns jcs = "`[`http://xml.juniper.net/junos/commit-scripts/1.0`](http://xml.juniper.net/junos/commit-scripts/1.0)`";`
`import "../import/junos.xsl";`
`match configuration {`
`    var $hostname = system/host-name;`
`    call jcs:emit-change($tag = 'transient-change', $dot = .) {`
`        with $content = {`
`            `<groups>` {`
`                `<name>` "re0";`
`                `<system>` {`
`                    `<host-name>` "re0." _ $hostname;`
`                }`
`            }`
`            `<groups>` {`
`                `<name>` "re1";`
`                `<system>` {`
`                    `<host-name>` "re1." _ $hostname;`
`                }`
`            }`
`            `<system>` {`
`                `<host-name inactive="inactive">`;`
`            }`
`            `<apply-groups>` "re0";`
`            `<apply-groups>` "re1";`
`        }`
`    }`
`}`