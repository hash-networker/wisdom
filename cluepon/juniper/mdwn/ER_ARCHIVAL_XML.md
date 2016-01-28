---
title: ER ARCHIVAL XML
permalink: /ER_ARCHIVAL_XML/
---

Problem
-------

Junos allow configuration to be saved to a remote ftp server on commit or regular basis.

` system {`
`     archival {`
`         configuration {`
`             transfer-on-commit;`
`             archive-sites {`
`                 "`[`ftp://user:pass@server/folder-for-router/`](ftp://user:pass@server/folder-for-router/)`";`
`             }`
`         }`
`     }`
` }`

The file saved is a dump of the "show configuration" which is not friendly for parsing with tools.

Allow to specify that the configuration should be saved in xml, resulting in a file equivalent to "show configuration | display commit-scripts view" or "show configuration | display xml"

Work Around
-----------

This is possible using event-options :

` event-options {`
`     policy archive-configuration {`
`         events ui_commit;`
`         then {`
`             execute-commands {`
`                 commands {`
`                     "show configuration | display commit-scripts view";`
`                 }`
`                 output-filename xml-conf;`
`                 destination ftp-server;`
`                 output-format xml;`
`             }`
`         }`
`     }`
`     destinations {`
`         ftp-server {`
`             transfer-delay 60;`
`             archive-sites {`
`                 "`[`ftp://user@server/a/folder/`](ftp://user@server/a/folder/)`" password "the password encrypted";`
`             }`
`         }`
`     }`
` }`

For some reason the event is not generated reliably on some JunOS version, the first working version I found was 8.3R3.6 (but it may be working with previous version depending on your luck).

Unlike with the ftp configuration which retry the upload if the destination is unreacheable, the event based work around seems to only perform a one shot upload attempt.

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")