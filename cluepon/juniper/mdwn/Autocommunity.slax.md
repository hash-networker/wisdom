---
title: Autocommunity.slax
permalink: /Autocommunity.slax/
---

`version 1.0;`
`ns junos = "`[`http://xml.juniper.net/junos/*/junos`](http://xml.juniper.net/junos/*/junos)`";`
`ns xnm = "`[`http://xml.juniper.net/xnm/1.1/xnm`](http://xml.juniper.net/xnm/1.1/xnm)`";`
`ns jcs = "`[`http://xml.juniper.net/junos/commit-scripts/1.0`](http://xml.juniper.net/junos/commit-scripts/1.0)`";`
`import "../import/junos.xsl";`
`template comm_action($asn, $name, $continent, $region, $city)`
`{`
`    var $reg_c    = "(0" _ $continent _ "0)";`
`    var $reg_cr   = "(0" _ $continent _ $region _ ")";`
`    var $reg_city = "(1" _ $city _ ")";`
`    var $regexp   = "((000)|" _ $reg_c _ "|" _ $reg_cr _ "|" _ $reg_city _ ")$";`
`    `<transient-change>` {`
`        `<policy-options>` {`
`            /* Generate Communities */`
`            `<community>` {`
`                `<name>` "MATCH_" _ $name _ "_PREPEND_ONE";`
`                `<members>` "^" _ $asn _ ":1" _ $regexp;`
`            }`
`            `<community>` {`
`                `<name>` "MATCH_" _ $name _ "_PREPEND_TWO";`
`                `<members>` "^" _ $asn _ ":2" _ $regexp;`
`            }`
`            `<community>` {`
`                `<name>` "MATCH_" _ $name _ "_PREPEND_THREE";`
`                `<members>` "^" _ $asn _ ":3" _ $regexp;`
`            }`
`            `<community>` {`
`                `<name>` "MATCH_" _ $name _ "_PREPEND_FOUR";`
`                `<members>` "^" _ $asn _ ":4" _ $regexp;`
`            }`
`            `<community>` {`
`                `<name>` "MATCH_" _ $name _ "_MED_ZERO";`
`                `<members>` "^" _ $asn _ ":5" _ $regexp;`
`            }`
`            `<community>` {`
`                `<name>` "MATCH_" _ $name _ "_DENY_EXPORT";`
`                `<members>` "^" _ $asn _ ":6" _ $regexp;`
`            }`
`            `<community>` {`
`                `<name>` "MATCH_" _ $name _ "_FORCE_EXPORT";`
`                `<members>` "^" _ $asn _ ":7" _ $regexp;`
`            }`
`            if ($name == "GLOBAL") {`
`                `<community>` {`
`                    `<name>` "MATCH_" _ $name _ "_DENY_IMPORT";`
`                    `<members>` "^" _ $asn _ ":8" _ $regexp;`
`                }`
`            }`
`            /* Generate Policies referencing Communities */`
`            `<policy-statement>` {`
`                `<name>` "AUTO-COMMUNITY-" _ $name _ "-IN";`
`                `<then>` {`
`                    `<next>` "policy";`
`                }`
`            }`
`            `<policy-statement>` {`
`                `<name>` "AUTO-COMMUNITY-" _ $name _ "-OUT";`
`                `<term>` {`
`                    `<name>` "PREPEND_ONE";`
`                    `<from>` {`
`                        `<community>` "MATCH_" _ $name _ "_PREPEND_ONE";`
`                    }`
`                    `<then>` {`
`                        `<as-path-prepend>` "1234";`
`                    }`
`                }`
`                `<term>` {`
`                    `<name>` "PREPEND_TWO";`
`                    `<from>` {`
`                        `<community>` "MATCH_" _ $name _ "_PREPEND_TWO";`
`                    }`
`                    `<then>` {`
`                        `<as-path-prepend>` "1234 1234";`
`                    }`
`                }`
`                `<term>` {`
`                    `<name>` "PREPEND_THREE";`
`                    `<from>` {`
`                        `<community>` "MATCH_" _ $name _ "_PREPEND_THREE";`
`                    }`
`                    `<then>` {`
`                        `<as-path-prepend>` "1234 1234 1234";`
`                    }`
`                }`
`                `<term>` {`
`                    `<name>` "PREPEND_FOUR";`
`                    `<from>` {`
`                        `<community>` "MATCH_" _ $name _ "_PREPEND_FOUR";`
`                    }`
`                    `<then>` {`
`                        `<as-path-prepend>` "1234 1234 1234 1234";`
`                    }`
`                }`
`                `<term>` {`
`                    `<name>` "MED_ZERO";`
`                    `<from>` {`
`                        `<community>` "MATCH_" _ $name _ "_MED_ZERO";`
`                    }`
`                    `<then>` {`
`                        `<metric>` {`
`                            `<metric>` "0";`
`                        }`
`                    }`
`                }`
`                `<term>` {`
`                    `<name>` "DENY_EXPORT";`
`                    `<from>` {`
`                        `<community>` "MATCH_" _ $name _ "_DENY_EXPORT";`
`                    }`
`                    `<then>` {`
`                        `<default-action>` "reject";`
`                    }`
`                }`
`                `<term>` {`
`                    `<name>` "FORCE_EXPORT";`
`                    `<from>` {`
`                        `<community>` "MATCH_" _ $name _ "_FORCE_EXPORT";`
`                    }`
`                    `<then>` {`
`                        `<default-action>` "accept";`
`                    }`
`                }`
`                `<then>` {`
`                    `<next>` "policy";`
`                }`
`            }`
`        }`
`    }`
`}`
`template comm_info($asn, $continent, $region, $city)`
`{`
`    `<transient-change>` {`
`        `<policy-options>` {`
`            `<community>` {`
`                `<name>` "MATCH_CURRENT_CITY";`
`                `<members>` "^" _ $asn _ ":..." _ $city _ "$";`
`            }`
`            `<community>` {`
`                `<name>` "MATCH_CURRENT_REGION";`
`                `<members>` "^" _ $asn _ ":." _ $continent _ $region _ "..$";`
`            }`
`            `<community>` {`
`                `<name>` "TAG_TRANSIT";`
`                `<members>` $asn _ ":1" _ $continent _ $region _ $city;`
`            }`
`            `<community>` {`
`                `<name>` "TAG_PUBPEER";`
`                `<members>` $asn _ ":2" _ $continent _ $region _ $city;`
`            }`
`            `<community>` {`
`                `<name>` "TAG_PRIVPEER";`
`                `<members>` $asn _ ":3" _ $continent _ $region _ $city;`
`            }`
`            `<community>` {`
`                `<name>` "TAG_CUSTOMER";`
`                `<members>` $asn _ ":4" _ $continent _ $region _ $city;`
`            }`
`            `<community>` {`
`                `<name>` "TAG_INTERNAL";`
`                `<members>` $asn _ ":5" _ $continent _ $region _ $city;`
`            }`
`        }`
`    }`
`}`
`match configuration {`
`    var $path = /commit-script-input/configuration/policy-options;`
`    var $local-as = routing-options/autonomous-system/as-number;`
`    var $location = system/location/apply-macro[name = 'bgp'];`
`    var $continent = $location/data[name = 'continent']/value;`
`    var $region = $location/data[name = 'region']/value;`
`    var $city = $location/data[name = 'city']/value;`
`    /* Generate Information Communities */`
`    call comm_info($asn = $local-as, $continent, $region, $city);`
`    /* Generate Global Action Communities */`
`    call comm_action($asn = $local-as, $name = "GLOBAL", $continent, $region, $city);`
`    call comm_action($asn = "65001", $name = "TRANSIT", $continent, $region, $city);`
`    call comm_action($asn = "65002", $name = "PEER", $continent, $region, $city);`
`    call comm_action($asn = "65003", $name = "CUST", $continent, $region, $city);`
`    /* Generate Per-ASN Action Communities from configured BGP neighbors */`
`    for-each (protocols/bgp/group/neighbor[peer-as != $local-as]) {`
`        call comm_action($asn = peer-as, $name = peer-as, $continent, $region, $city);`
`        /* Insert into second position within import/export chain */`
`        var $import = jcs:first-of(import, ../import, ../../import);`
`        var $export = jcs:first-of(export, ../export, ../../export);`
`        var $community_import = "AUTO-COMMUNITY-" _ peer-as _ "-IN";`
`        var $community_export = "AUTO-COMMUNITY-" _ peer-as _ "-OUT";`
`        call jcs:emit-change($tag = 'transient-change') {`
`            with $content = {`
`                for-each ($import) {`
`                    `<import>` .;`
`                }`
`                for-each ($export) {`
`                    `<export>` .;`
`                }`
`                `<import insert="after" name=$import[1]>` $community_import;`
`                `<export insert="after" name=$export[1]>` $community_export;`
`            }`
`        }`
`    }`
`}`