---
title: ER Configuration includes
permalink: /ER_Configuration_includes/
---

Problem
-------

JunOS does not allow for configuration options like 'include <file_path>'. Often, a large network has common prefix-lists, firewall filters, static routes, or SNMP settings configured on many different routers. Currently, one has to rely on externally created software to make mass configuration changes when, for example, a common prefix-list needs to be updated.

Solution
--------

JunOS could be modified to allow the inclusion of external text files as such:

`prefix-lists {`
`     bogons {`
`          include {`
`              remote-path "`[`https://files.foo.com/bogon_list.txt`](https://files.foo.com/bogon_list.txt)`";`
`              local-path "/config/includes/";`
`     }`
`}`

Upon a 'commit', JunOS could fetch the specified file, copy it to the local directory, check syntax, and apply it to the config:

`[edit]`
`user@core2.iad# commit `
`fetch: `[`https://files.foo.com/bogon_list.txt`](https://files.foo.com/bogon_list.txt)` succeeded`
`commit complete`
`[edit]`
`user@core2.iad#`

If for some reason the URL is not available, JunOS can check to see if the local copy exists and apply that to the config instead, and issue a warning:

`[edit]`
`user@core2.iad# commit `
`fetch: `[`https://files.foo.com/bogon_list.txt`](https://files.foo.com/bogon_list.txt)` failed`
`[edit policy-options prefix-list bogons]`
`  'remote-path "`[`https://files.foo.com/bogon_list.txt`](https://files.foo.com/bogon_list.txt)`"' cannot be contacted; using local copy`
`commit complete`
`[edit]`
`user@core2.iad#`

or, fail if neither copy is retrievable:

`[edit]`
`user@core2.iad# commit `
`fetch: `[`https://files.foo.com/bogon_list.txt`](https://files.foo.com/bogon_list.txt)` failed`
`[edit policy-options prefix-list bogons]`
`  'remote-path "`[`https://files.foo.com/bogon_list.txt`](https://files.foo.com/bogon_list.txt)`"' cannot be contacted; local copy doesn't exist`
`error: configuration check-out failed`
`[edit]`
`user@core2.iad#`

Status
------

A solution can be achieved with Junos 7.4 using a [Commit Scripts](http://www.juniper.net/techpubs/software/junos/junos74/swconfig74-scripts/html/commit-scripts-overview.html#1033561).

`system {`
`    scripts {`
`        commit {`
`            allow-transients;`
`            file `<filename.xsl>` {`
`                refresh;`
`                refresh-from `<url>`;`
`            }`
`        }`
`    }`
`}`