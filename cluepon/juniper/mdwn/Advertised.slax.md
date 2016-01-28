---
title: Advertised.slax
permalink: /Advertised.slax/
---

`version 1.0;`
`ns junos = "`[`http://xml.juniper.net/junos/*/junos`](http://xml.juniper.net/junos/*/junos)`";`
`ns xnm = "`[`http://xml.juniper.net/xnm/1.1/xnm`](http://xml.juniper.net/xnm/1.1/xnm)`";`
`ns jcs = "`[`http://xml.juniper.net/junos/commit-scripts/1.0`](http://xml.juniper.net/junos/commit-scripts/1.0)`";`
`import "../import/junos.xsl";`
`var $arguments = {`
`    `<argument>` {`
`        `<name>` "prefix";`
`        `<description>` "Prefix to examine";`
`    }`
`}`
`param $prefix;`
`match / {`
`    var $neighbors = jcs:invoke('get-bgp-summary-information');`
`    `<op-script-results>` {`
`        `

<output>
"Prefix " _ $prefix _ " is advertised to the following BGP neighbors:";

`        for-each ($neighbors/bgp-peer[peer-state == 'Established']) {`
`            var $show_advertised = {`
`                `<get-route-information>` {`
`                    `<advertising-protocol-name>` "bgp";`
`                    `<neighbor>` peer-address;`
`                    `<destination>` $prefix;`
`                    `<terse>`;`
`                    `<exact>`;`
`                }`
`            };`
`            var $results = jcs:invoke($show_advertised);`
`            if ($results/route-table/rt/rt-destination) {`
`                `

<output>
" \* " _ peer-address _ " (" _ description _ ")";

`            }`
`        }`
`    }`
`}`