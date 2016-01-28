---
title: CS Simple AS-PATH Generator
permalink: /CS_Simple_AS-PATH_Generator/
---

Information
-----------

This script searches for any AS-PATH references in policy-statements of the form "AS\#\#\#\#" and automatically creates the named as-path reference for ".\*\#\#\#\#.\*".

Usage
-----

`policy-options {`
`    policy-statement example {`
`        term example {`
`            from as-path AS1234;`
`            then whatever;`
`            }`
`        }`
`    }`
`}`

Will automatically create:

`policy-options {`
`    as-path AS1234 ".*1234.*";`
`}`

Desperate Hack for Missing Feature
----------------------------------

[Enhancement Request - Numeric AS-PATH References](/ER_Unnamed_AS-PATH_and_Community_references "wikilink")

ChangeLog
---------

1.0 - Initial Version