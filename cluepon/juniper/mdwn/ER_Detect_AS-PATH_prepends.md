---
title: ER Detect AS-PATH prepends
permalink: /ER_Detect_AS-PATH_prepends/
---

Problem
-------

AS-PATH prepending is commonly used to steer ingress traffic away from a given transit provider. However, other networks' local-preference overrides this quite handily. You can detect prepends for known ASNs through existing policy language features, but you must specify the ASNs, resulting in slow, unwieldy as-path-groups, or limiting your policy to detecting prepends of only a few ASNs.

`policy-options {`
`    as-path-group PREPEND1 {`
`        as-path 3356 ".* 3356 3356 .*";`
`        as-path 174 ".* 174 174 .*";`
`    }`
`    as-path-group PREPEND2 {`
`        as-path 3356 ".* 3356 3356 3356 .*";`
`        as-path 174 ".* 174 174 174 .*";`
`    }`
`    policy-statement TRANSIT-IMPORT {`
`        /* subtract 5 from local-preference when we see certain AS-PATH prepend sequences */`
`        term DETECT-PREPEND1 {`
`            from as-path-group PREPEND1;`
`            then local-preference subtract 5;`
`        }`
`        /* subtract 5 more if detected prepended ASN is seen three times */`
`        term DETECT-PREPEND2 {`
`            from as-path-group PREPEND2;`
`            then local-preference subtract 5;`
`        }`
`    }`
`}`

Solution
--------

Extend the functionality of AS-PATH regular expressions to include the ability to match on substrings already seen in the string representation of the path.

`policy-options {`
`    as-path PREPEND1 ".* (.) $1 .*";`
`    as-path PREPEND2 ".* (.) $1 $1 .*";`
`    policy-statement TRANSIT-IMPORT {`
`        /* subtract 5 from local-preference if AS-PATH prepending is seen */`
`        term DETECT-PREPEND1 {`
`            from as-path PREPEND1;`
`            then local-preference subtract 5;`
`        }`
`        /* subtract 5 more if the same ASN is seen three times */`
`        term DETECT-PREPEND2 {`
`            from as-path PREPEND2;`
`            then local-preference subtract 5;`
`        }`
`    }`
`}`

Status
------

Unknown.