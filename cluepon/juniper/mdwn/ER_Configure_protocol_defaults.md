---
title: ER Configure protocol defaults
permalink: /ER_Configure_protocol_defaults/
---

Problem
-------

[JunOS](/JunOS "wikilink") has different built-in default policy termination actions for import/export policies based on the protocol in question, which are not configurable. For example, the following configuration:

`policy-options {`
`    policy-statement example-export-policy {`
`        term example {`
`            from match_criteria;`
`            then reject;`
`        }`
`    }`
`}`
`protocols {`
`    bgp {`
`        group example {`
`            neighbor 1.2.3.4 {`
`                remote-as 1234;`
`                export example-export-policy;`
`            }`
`        }`
`    }`
`}`

will allow any route which does not match the term "example", because the default BGP action for import and export is to accept the route. While these per-protocol default policy termination actions are documented, they can not be changed. It is the responsibility of the policy-statement to reject any routes which should not be imported or exported.

Solution
--------

Allowing the default policy termination action to be configured on a per-protocol basis would allow power users to optionally configure their import or export defaults to reject routes, rather than accept them. This could be used to prevent accidental routing leaks due to policy-statement mistakes, by erring in favor of rejecting the route if not explicitly permitted.

For example:

`protocols {`
`    bgp {`
`        default-action reject;`
`    }`
`}`

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")