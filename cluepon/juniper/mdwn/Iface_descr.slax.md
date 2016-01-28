---
title: Iface descr.slax
permalink: /Iface_descr.slax/
---

`version 1.0;`
`ns junos = "`[`http://xml.juniper.net/junos/*/junos`](http://xml.juniper.net/junos/*/junos)`";`
`ns xnm = "`[`http://xml.juniper.net/xnm/1.1/xnm`](http://xml.juniper.net/xnm/1.1/xnm)`";`
`ns jcs = "`[`http://xml.juniper.net/junos/commit-scripts/1.0`](http://xml.juniper.net/junos/commit-scripts/1.0)`";`
`import "../import/junos.xsl";`
`template interface_comment($iface, $comment)`
`{`
`    /* Find all references to the interface name under "protocols" */`
`    for-each (//protocols//interface[name == $iface]) {`
`        /* "annotate" a comment onto the interface references */`
`        call jcs:emit-change($dot = ..) {`
`            with $content = {`
`                `<junos:comment replace="replace">` $comment;`
`                `<interface>` {`
`                    `<name>` $iface;`
`                }`
`            }`
`        }`
`    }`
`}`
`match configuration {`
`    /* Scan interface configurations */`
`    for-each (interfaces/interface) {`
`        var $int_comment = jcs:first-of(description, "No Description");`
`        /* Comment on Physical Interface references */`
`        call interface_comment($iface = name, $comment = $int_comment);`
`        /* Comment on Subinterface references */`
`        for-each (unit) {`
`            var $iface = concat(../name, ".", name);`
`            var $comment = jcs:first-of(description, ../description);`
`            if (string-length($comment)) {`
`                call interface_comment($iface, $comment);`
`            } else {`
`                if (name != "0") {`
`                    /* If subint isn't 0, set a "No Description" description */`
`                    call interface_comment($iface, $comment = "No Description");`
`                }`
`            }`
`        }`
`    }`
`}`