---
title: Automesh.slax
permalink: /Automesh.slax/
---

`version 1.0;`
`ns junos = "`[`http://xml.juniper.net/junos/*/junos`](http://xml.juniper.net/junos/*/junos)`";`
`ns xnm = "`[`http://xml.juniper.net/xnm/1.1/xnm`](http://xml.juniper.net/xnm/1.1/xnm)`";`
`ns jcs = "`[`http://xml.juniper.net/junos/commit-scripts/1.0`](http://xml.juniper.net/junos/commit-scripts/1.0)`";`
`import "../import/junos.xsl";`
`match configuration {`
`    var $hostname = system/host-name;`
`    for-each (protocols/mpls/apply-macro[name = 'automesh']/data) {`
`        if (name != $hostname) {`
`            call jcs:emit-change($dot = ../..) {`
`                with $content = {`
`                    `<label-switched-path>` {`
`                        `<name>` {`
`                            /* Ghetto hack to translate to uppercase */`
`                            var $lowercase = "abcdefghijklmnopqrstuvwxyz";`
`                            var $uppercase = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";`
`                            expr translate($hostname, $lowercase, $uppercase);`
`                            expr "-";`
`                            expr translate(name, $lowercase, $uppercase);`
`                        }`
`                        `<to>` value;`
`                    }`
`                }`
`            }`
`        }`
`    }`
`}`