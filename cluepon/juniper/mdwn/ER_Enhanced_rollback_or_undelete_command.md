---
title: ER Enhanced rollback or undelete command
permalink: /ER_Enhanced_rollback_or_undelete_command/
---

Problem
-------

JUNOS software provides an extremely powerful set of commands to manage configuration changes, and to manage reverting to previous configurations when necessary. However, one area which is missing is that of a partial rollback, or "undelete" command.

Currently, if you delete a portion of your configuration (such as when doing a cleanup of a router configuration) and later discover that you would like to restore that portion of the configuration, you must either abandon the current candidate configuration completely (rollback) and start over, or attempt to locate the specific configuration which has been changed and re-apply it. An example of the commands necessary to do this is:

`[edit]`
`user@router# commit check `
`[edit]`
`  'policy-options'`
`    Policy error: EXAMPLE prefix-list referenced (in term example) but not defined`
`[edit]`
`user@router# edit policy-options `
`[edit policy-options]`
`user@router# show | compare rollback 0`
`...`
`-  prefix-list EXAMPLE {`
`-      1.2.3.0/24;`
`-      2.3.4.0/24;`
`-      3.4.0.0/18;`
`-  }`
`...`
`[edit policy-options]`
`user@router# load merge terminal relative`
`  prefix-list EXAMPLE {`
`      1.2.3.0/24;`
`      2.3.4.0/24;`
`      3.4.0.0/18;`
`  }`

This process can be time-consuming in the event of a large set of changes in your candidate configuration, and requires that you place the subsection of configuration into an external text editor to remove the leading "-" character from the compare diff.

Solution 1
----------

Implement a command which allows users to directly recover portions of the configuration from the previous version (or an arbitary version). An example of such a command to restore whole configuration blocks might be an "undelete" command.

For example, to restore portions of the candidate configuration from the previously committed version:

`[edit policy-options]`
`user@router# undelete prefix-list EXAMPLE`

Another example may be to restore portions of the candidate configuration from any arbitrary configuration version:

`[edit policy-options]`
`user@router# undelete rollback 1 prefix-list EXAMPLE`

Solution 2
----------

Extend the "rollback" command to work with sub-sections of the configuration. For example:

`[edit policy-options]`
`user@router# rollback 0 prefix-list EXAMPLE`

`[edit policy-options]`
`user@router# rollback 0 prefix-list EXAMPLE`

`[edit]`
`user@router# rollback 1 policy-options`

Other Enhancements
------------------

-   An optional "detail" command which displays the diff of what changes have occured in the rollback.
-   Displaying which sub-sections of the configuration have changes with a "?" command in the CLI.

For example:

`[edit policy-options]`
`user@router# rollback 0 ?`
`Possible completions:`
`  <[Enter]>            Execute this command`
`  prefix-list EXAMPLE  Changed at 2007-02-18 22:58:43 EST by user via cli`
`  ...`

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")