---
title: ER Firewall default term
permalink: /ER_Firewall_default_term/
---

Problem
-------

Often, a firewall filter will be configured with a default action, such as "accept" or "discard", for packets which do not match any previous terms. A way to express a "default term" or "default action" which always remains at the end of the ruleset when adding new terms is considered very convenient. Juniper implements this concept under its "policy-statement" syntax, with a "from" and "then" statement outside of any term:

`policy-options {`
`    policy-statement foo {`
`        term term_a {`
`            from match_criteria;`
`            then match_action;`
`        }`
`        term term_b {`
`            from match_criteria;`
`            then match_action;`
`        }`
`        ...`
`        then {`
`            default_actions;`
`        }`
`    }`
`}`

However, Juniper's firewall syntax lacks this concept. Currently the only way to implement this is:

`firewall {`
`    filter example {`
`         term term_a {`
`             from match_criteria;`
`             then match_action;`
`         }`
`         term term_b {`
`             from match_criteria;`
`             then match_action;`
`         }`
`         ...`
`         term default {`
`             then discard;`
`         }`
`    }`
`}`

Under this system, when a user adds a new term the term is automatically inserted at the end, requiring the user to do:

`edit firewall filter example`
`set term new_term ...`
`insert term new_term before term default`

Solutions
---------

By adding this concept, in the same way that it is implemented under "policy-statement", users would be able to explicitly set a "default action" which they know will always stay at the bottom of the filter as the "final term". This would simplify the use of firewalls, make their use more consistent with other portions of JUNOS, and prevent accidental misconfiguration of firewall filters.

`firewall {`
`    filter example {`
`         term term_a {`
`             from match_criteria;`
`             then match_action;`
`         }`
`         term term_b {`
`             from match_criteria;`
`             then match_action;`
`         }`
`         ...`
`         then {`
`             discard;`
`         }`
`    }`
`}`

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")