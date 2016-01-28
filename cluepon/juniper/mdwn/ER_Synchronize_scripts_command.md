---
title: ER Synchronize scripts command
permalink: /ER_Synchronize_scripts_command/
---

Problem
-------

Currently, [JUNOS](/JUNOS "wikilink") has a mechanism to manually update commit, op, and event scripts (XSLT/SLAX files) from a specified source:

`[edit]`
`user@router# show system scripts commit `
`allow-transients;`
`file example.slax {`
`    source "`[`http://www.example.com/example.slax`](http://www.example.com/example.slax)`";`
`}`
`[edit]`
`user@router# set system scripts commit refresh `
`refreshing 'example.slax' from '`[`http://www.example.com/example.slax`](http://www.example.com/example.slax)`'`
`/var/home/remote/...transferring.file.........lxODvZ/example.slax100% of 8468  B 4674 kBps`

However, there is no mechanism to synchronize these files between the primary and backup Routing Engines. If you do not have an IP address configured and accessable for both Routing Engines, you may not be able to update the backup RE at all. Further, if you make a configuration change which affects these scripts and "commit synchronize", the configuration is copied to the backup RE but the new scripts are not, potentially resulting in an error which corrupts the backup configuration or prevents the commit from occuring.

The current solution requires you to manually synchronize the files after every update, like this:

`[edit]`
`user@router# run copy file /var/db/scripts/commit/example.slax re1:/var/db/scripts/commit`

Solution
--------

Add a command to automatically synchronize the script files between routing engines. For example:

Allow the SNMP configuration to reference prefix-lists, as well as manually entered IP prefixes.

For example:

`[edit]`
`user@router# commit synchronize scripts`

`[edit]`
`user@router# commit synchronize scripts commit`

And automatic synchronization support:

`system {`
`    commit {`
`        synchronize {`
`            scripts {`
`                commit;`
`            }`
`        }`
`    }`
`}`

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")