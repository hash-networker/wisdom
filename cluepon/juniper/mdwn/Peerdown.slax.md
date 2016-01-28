---
title: Peerdown.slax
permalink: /Peerdown.slax/
---

`version 1.1;`
`/* `
` * $Id: peerdown.slax 994 2010-03-15 12:58:45Z teun $ `
` * `
` * This operation script shows information for all peerings`
` * which are down.`
` *`
` * Author: Teun Vink `<teun@bit.nl>
` * `
` */`
`ns junos = "`[`http://xml.juniper.net/junos/*/junos`](http://xml.juniper.net/junos/*/junos)`";`
`ns xnm = "`[`http://xml.juniper.net/xnm/1.1/xnm`](http://xml.juniper.net/xnm/1.1/xnm)`";`
`ns jcs = "`[`http://xml.juniper.net/junos/commit-scripts/1.0`](http://xml.juniper.net/junos/commit-scripts/1.0)`";`
`import "../import/junos.xsl";`
`match / `
`{`
`    `<op-script-results>` `
`    {`
`       /* execute the 'show bgp summary' command and store the result */`
`       var $query = { `<command>` 'show bgp summary'; }`
`       var $result = jcs:invoke($query);`
`       /* print a header */`
`       `

<output>
jcs:printf("%-34s %10s %15s %s", "peer IP", "peer ASN", "downtime", "peer description");

<output>
jcs:printf("==================================================================================");

`       /* search for the bgp information tag */`
`       `<bgp-information>` {`
`           /* print each peer which's state is not Established */`
`           for-each($result/bgp-peer[peer-state!='Established']) {`
`                `

<output>
jcs:printf("%-35s%10s %15s %s", peer-address, peer-as, elapsed-time, description);

`           }`
`       }`
`    }`
`}`