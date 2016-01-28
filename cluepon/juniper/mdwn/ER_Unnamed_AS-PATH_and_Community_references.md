---
title: ER Unnamed AS-PATH and Community references
permalink: /ER_Unnamed_AS-PATH_and_Community_references/
---

Problem
-------

Currently, [JUNOS](/JUNOS "wikilink") requires that AS-PATH and BGP Community definitions be named, and that you reference those elements by name within policy-statements. While this makes sense for many configurations where you want to re-use the AS-PATH or Community definitions, it doesn't make sense for every configuration. When the element is referenced only once, for example in its use as a "quick hack", creating a named element clutters the configuration and adds unnecessary steps across multiple configuration levels.

For example:

`policy-option {`
`    policy-statement example {`
`        term example {`
`            from {`
`                as-path aspath_name;`
`                community community_match_name;`
`            }`
`            then community add community_add_name;`
`            ...`
`        }`
`    }`
`    as-path aspath_name ".*1234.*";`
`    community match_name members 1234:[12345]...;`
`    community add_name members 1234:1111;`
`}`

Solution
--------

Allow users to directly configure simple AS-PATH and BGP Community elements without creating and naming them under the policy-options level. The implementation is fairly straight forward, functioning similarly to "compiler generated variable names".

For example:

`policy-option {`
`    policy-statement example {`
`        term example {`
`            from {`
`                as-path ".*1234.*";`
`                community 1234:1111;`
`            }`
`            ...`
`        }`
`    }`
`}`

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")