---
title: ER Enhance BGP summary
permalink: /ER_Enhance_BGP_summary/
---

Problem
-------

The output of the JUNOS command "show bgp summary", which is the primary command for determining the status of a BGP session or sessions (including the current connection state, length of time up/down, and number of prefixes accepted/received), can be difficult to read on a router with many BGP sessions. When Multiprotocol BGP sessions are configured, the data is split onto multiple lines, making parsing through "| match" options even more difficult. An example of this type of output is:

`1.2.3.4          1234    1304483    2313874      18       6     9w5d20h Establ`
`  inet.0: 10582/17068/0`
`  inet.2: 1/1/0`
`  inet6.0: 2/130/0`
`  inet6.2: 0/0/0`

Solutions
---------

The "show bgp summary" command could be greatly improved with the addition of new options to search and/or sort the output. Some possible useful options include:

Show only BGP sessions configured to the remote-as "1234":

`user@router> show bgp summary remote-as 1234`

Show only BGP sessions which are Established:

`user@router> show bgp summary state established`

Show only BGP sessions which are not Established:

`user@router> show bgp summary state not-established`

Show only BGP sessions which are exchanging "inet6-unicast" NLRIs:

`user@router> show bgp summary nlri inet6-unicast`

Sort output in order of the configured BGP neighbor IP:

`user@router> show bgp summary sort neighbor-ip`

Sort output in order of the configured BGP remote-as:

`user@router> show bgp summary sort remote-as`

Sort output in order of the session uptime/downtime:

`user@router> show bgp summary sort uptime`

Other useful search/sort options may exist as well.

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")