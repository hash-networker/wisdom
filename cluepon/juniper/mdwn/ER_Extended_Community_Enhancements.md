---
title: ER Extended Community Enhancements
permalink: /ER_Extended_Community_Enhancements/
---

Problem
-------

Juniper BGP Extended Communities implementation is currently missing two critical features necessary for adoption and widespread use as a replacement for classic 32-bit BGP communities.

-   Regular Expression support
-   A mechanism to copy data between regular and extended communities.

Support for Regular Expressions is straightforward, and should be implemented immediately. It is clear that the existing implementation has not been targetting BGP Extended Communities as a replacement for regular communities, and this should be rectified.

A mechanism to copy data between regular and extended communities is essential to promote compatibility between the two formats and facilitate the widespread deployment of Extended Communities on the Internet. The most important extended community types to support are the transitive two-octet AS specific Route Target and Route Origin types. When copying data from regular to extended, the first half of the regular community would be mapped to the administrator field, and the second half of the regular community would be mapped to the value field. When copying from extended to regular communities, the operation would function in reverse, with an additional step of truncating the 32-bit administrator field to the lower 16-bits before copying to the regular community.

In addition to providing support for policy communities with 32-bit ASNs, this mapping system would effectively expand the policy functions of BGP communities by utilizing the target/origin distinguishers as defined by Extended Communities. Origin type communities could be used to provide information from a particular ASN and Target type communities could be used to apply actions to a particular ASN, without the risk of conflicting policy definitions. This problem is outlined in slide 28 of <http://www.nanog.org/mtg-0706/Presentations/BGPcommunities.pdf>.

Solutions
---------

A possible implementation mechanism could be:

`policy-options {`
`    policy-statement {`
`        term copy-example {`
`            from {`
`                optional match for specific routes and communities`
`            }`
`            then community convert regular-extended target;`
`        }`
`    }`
`}`

This policy would convert any existing communities on any routes matched in the from section into extended communities of type target. For example, a regular community "1234:45678" on a matched route would be converted to "target:1234:45678".

When going in the opposite direction:

`policy-options {`
`    policy-statement {`
`        term copy-example {`
`            from {`
`                optional match for specific routes and communities`
`            }`
`            then community convert extended-regular;`
`        }`
`    }`
`}`

The community target:1234:45678 would be converted back to 1234:45678. An optional command to delete or not delete the old community after the conversion would be helpful as well.

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")