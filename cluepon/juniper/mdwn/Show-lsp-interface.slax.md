---
title: Show-lsp-interface.slax
permalink: /Show-lsp-interface.slax/
---

`/*`
` * Copyright (c) 2007-2009 Cougar `<cougar@random.ee>
` *`
` * Permission to use, copy, modify, and distribute this software for any`
` * purpose with or without fee is hereby granted, provided that the above`
` * copyright notice and this permission notice appear in all copies.`
` *`
` * THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES`
` * WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF`
` * MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR`
` * ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES`
` * WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN`
` * ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF`
` * OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.`
` */`
`version 1.0;`
`ns junos = "`[`http://xml.juniper.net/junos/*/junos`](http://xml.juniper.net/junos/*/junos)`";`
`ns xnm = "`[`http://xml.juniper.net/xnm/1.1/xnm`](http://xml.juniper.net/xnm/1.1/xnm)`";`
`ns jcs = "`[`http://xml.juniper.net/junos/commit-scripts/1.0`](http://xml.juniper.net/junos/commit-scripts/1.0)`";`
`import "../import/junos.xsl";`
`var $arguments = {`
`       `<argument>` {`
`               `<name>` "interface";`
`               `<description>` "interface name";`
`       }`
`       `<argument>` {`
`               `<name>` "srcif";`
`               `<description>` "source interface name";`
`       }`
`       `<argument>` {`
`               `<name>` "dstif";`
`               `<description>` "destination interface name";`
`       }`
`       `<argument>` {`
`               `<name>` "showbypass";`
`               `<description>` "1 = show Bypass LSPs also";`
`       }`
`}`
`param $interface;`
`param $srcif;`
`param $dstif;`
`param $showbypass;`
`template resolve_ifdescr($ifdescrdb, $if)`
`{`
`       if ($if) {`
`               if ($ifdescrdb/logical-interface[name == $if]/description) {`
`                       expr $ifdescrdb/logical-interface[name == $if]/description;`
`               } else {`
`                       expr "-";       /* no descripion */`
`               }`
`       } else {`
`               expr "?";               /* no next-hop interface (ASSERT ?)*/`
`       }`
`}`
`template resolve_nh_ifdescr($ifdescrdb, $nhif)`
`{`
`  var $if = $nhif;`
`       call resolve_ifdescr($ifdescrdb, $if);`
`}`
`template resolve_ph_ifdescr($ifdescrdb, $phif)`
`{`
`  var $if = $phif;`
`       call resolve_ifdescr($ifdescrdb, $if);`
`}`
`template show_lsp_list()`
`{`
`       var $q1 = { `<command>` 'show mpls lsp detail'; }`
`       var $lspdb = jcs:invoke($q1);`
`       var $q2 = { `<command>` 'show interface descriptions'; }`
`       var $ifdescrdb = jcs:invoke($q2);`
`if ($srcif = '') {`
`       expr jcs:output('------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------');`
`       expr jcs:output("t name                                                                                                                              dst             next-hop        dst-if        dst-ifdescr\n");`
`       for-each($lspdb/rsvp-session-data[session-type == 'Ingress']/rsvp-session/mpls-lsp[lsp-state == 'Up']) {`
`               var $name = name;`
`               var $dst = destination-address;`
`               var $nhip = substring-before(substring-after(mpls-lsp-path/explicit-route, "\n"), "\n");`
`               var $routequery = { `<command>` 'show route table inet.0 protocol direct ' _ $nhip; }`
`               var $routeresult = jcs:invoke($routequery);`
`               var $nhif = $routeresult/route-table/rt/rt-entry/nh/via;`
`               if ((($interface == '') and ($dstif == '')) or ($interface == $nhif) or ($dstif == $nhif)) {`
`                       var $ndescr = { call resolve_nh_ifdescr($ifdescrdb, $nhif); }`
`                       var $outline = { expr jcs:printf("I %-32s                                                                                                  %-15s %-15s %-13s %-50s\n", $name, $dst, $nhip, $nhif, $ndescr); }`
`                       expr jcs:output($outline);`
`               }`
`       }`
`}`
`if ($dstif = '') {`
`       expr jcs:output('------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------');`
`       expr jcs:output("t name                             src             prev-hop        src-if        src-ifdescr\n");`
`       for-each($lspdb/rsvp-session-data[session-type == 'Egress']/rsvp-session[lsp-state == 'Up' and (not(starts-with(name, 'Bypass')) or ($showbypass == 1))]) {`
`               var $name = name;`
`               var $src = source-address;`
`               var $phip = packet-information/previous-hop;`
`               var $routequery = { `<command>` 'show route table inet.0 protocol direct ' _ $phip; }`
`               var $routeresult = jcs:invoke($routequery);`
`               var $phif = $routeresult/route-table/rt/rt-entry/nh/via;`
`               if ((($interface == '') and ($srcif == '')) or ($interface == $phif) or ($srcif == $phif)) {`
`                       var $pdescr = { call resolve_ph_ifdescr($ifdescrdb, $phif); }`
`                       var $outline = { expr jcs:printf("E %-32s %-15s %-15s %-13s %-50s\n", $name, $src, $phip, $phif, $pdescr); }`
`                       expr jcs:output($outline);`
`               }`
`       }`
`}`
`       expr jcs:output('------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------');`
`       expr jcs:output("t name                             src             prev-hop        src-if        src-ifdescr                                        dst             next-hop        dst-if        dst-ifdescr\n");`
`       for-each($lspdb/rsvp-session-data[session-type == 'Transit']/rsvp-session[lsp-state == 'Up' and (not(starts-with(name, 'Bypass')) or ($showbypass == 1))]) {`
`               var $name = name;`
`               var $src = source-address;`
`               var $dst = destination-address;`
`               if (packet-information[@heading == "  PATH"]/previous-hop) {`
`                       var $phip = packet-information[@heading == "  PATH"]/previous-hop;`
`                       var $phif = packet-information[previous-hop == $phip]/interface-name;`
`                       var $nhip = packet-information[@heading == "  PATH"]/next-hop;`
`                       var $nhif = packet-information[next-hop == $nhip]/interface-name;`
`                       if ((($interface == '') and ($srcif == '') and ($dstif == '')) or ($interface == $phif) or ($interface == $nhif) or (($srcif == $phif) and ($dstif == '')) or (($dstif == $nhif) and ($srcif == '')) or (($srcif == $phif) and ($dstif == $nhif))) {`
`                               var $pdescr = { call resolve_ph_ifdescr($ifdescrdb, $phif); }`
`                               var $ndescr = { call resolve_nh_ifdescr($ifdescrdb, $nhif); }`
`                               var $outline  = { expr jcs:printf("T %-32s %-15s %-15s %-13s %-50s %-15s %-15s %-13s %-50s\n", $name, $src, $phip, $phif, $pdescr, $dst, $nhip, $nhif, $ndescr); }`
`                               expr jcs:output($outline);`
`                       }`
`               }`
`       }`
`}`
`match / {`
`       `<op-script-results>` {`
`               call show_lsp_list();`
`       }`
`}`