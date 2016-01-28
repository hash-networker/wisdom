---
title: ER Configuration hiding
permalink: /ER_Configuration_hiding/
---

Problem
-------

[JUNOS](/JUNOS "wikilink") configurations can grow to be extremely large, especially when [JUNOScript](/JUNOScript "wikilink") or other script-based automated systems are used to deploy configuration to routers, resulting in configurations which are difficult to read or search through during normal operator use. In many instances, portions of this configuration can be safely suppressed from normal view. Examples of this include standardization configuration sections which are the same across all routers, and script-generated prefix-lists which are uses for filtering BGP customer announcements.

Solution
--------

Add a new "hidden" keyword and "hide" command which can prevent the display of these sections under normal circumstances. This could be overridden by piping the "show" command to a "display hidden" command.

For example:

`groups {`
`    generic-configurations {`
`        ...extremely long generic configurations...`
`    }`
`}`
`policy-options {`
`    prefix-list CUSTOMER:AS1234 {`
`        ...extremely long script-generated customer prefix-list...`
`    }`
`}`

Could be abbreviated as:

`groups {`
`    hidden: generic-configurations;`
`}`
`policy-options {`
`    hidden: prefix-list CUSTOMER:1234;`
`}`

Configuration sections could be hidden through a "hide" command:

`[edit policy-options]`
`user@router# hide prefix-list CUSTOMER:1234`

The complete configuration could be viewed manually using a new "display hidden" command:

`[edit]`
`user@router# show | display hidden`

Status
------

Added to JUNOS 8.2

Statement to limit configuration output—Enables you to limit output that appears when you use the show command to display a configuration. Include the apply-flags omit configuration statement at any hierarchy level to omit the containing hierarchy from show command output. This feature is useful for hiding portions of the configuration that include lengthy or redundant configuration statements. \[JUNOS CLI User Guide\]

[Category:Feature Requests](/Category:Feature_Requests "wikilink")