---
title: ER Configuration audit command
permalink: /ER_Configuration_audit_command/
---

Problem
-------

New engineers are often forced to take over Juniper router configurations from a previous party, through company acquisitions or replacing previous employees. Sometimes these configurations have not been maintained as well as they should have been, and can contain large numbers of elements which are difficult to track down and remove.

Solution
--------

Implement an "audit" command, which counts or displays the locations in which portions of the configuration that can be referenced elsewhere are actually referenced. This includes policy-statements, prefix-lists, communities, as-path definitions, firewall filters, and groups.

For example:

`user@router> request system configuration audit count`
`Policy Statements:`
`   example-policy-statement-A             1`
`   example-policy-statement-B             3`
`   example-policy-statement-C             2`
`   example-policy-statement-D             1`
`   example-policy-statement-E             0`
`   example-policy-statement-F             0`
`   example-policy-statement-G             0`
`   example-policy-statement-H             1`
`...`

`user@router> request system configuration audit prefix-list`
`   example-prefix-list-A`
`             [edit policy-options policy-statement EXAMPLE term example from]`
`             [edit policy-options policy-statement EXAMPLE2 term anotherexample from]`
`             [edit firewall filter EXAMPLE term example from]`
`   example-prefix-list-B`
`   example-prefix-list-C`
`...`

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")