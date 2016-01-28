---
title: OS shbgpsum
permalink: /OS_shbgpsum/
---

Information
-----------

The output of "show bgp summary" may be a little confusing if you have a lot of peers. This op script only shows informations for neighbors in a single ASN, completed with description and a separator. Also, matching the neighbour description is now supported (case insensitive). Then someone said "you could filter by state too!".

Usage
-----

Config as:

    me@router-re0> show configuration system scripts op
    file shbgpsum.slax {
        command bgpsum;
        description "Restrict show bgp summary to a single AS";
    }

Call it as:

    me@router-re0> op bgpsum asn 3333
    Peer                     AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dwn State|#Active/Received/Accepted/Damped...
    RIPE
    195.69.144.68          3333      35318     120919       0       0     1w0d22h Established
      inet.0: 6/6/6/0
    ----------
    RIPE 2
    195.69.144.71          3333      36528     120918       0       0     1w0d22h Established
      inet.0: 6/6/6/0
    ----------
    RIPE
    2001:7f8:1::a500:3333:1      3333      23801      24348       0       0     1w0d22h Established
      inet6.0: 1/1/1/0
    ----------
    RIPE 2
    2001:7f8:1::a500:3333:2      3333      23467      24348       0       0     1w0d22h Established
      inet6.0: 1/1/1/0
    ----------

    {master}

Or also as

    me@router-re0> op bgpsum name root
    Peer                     AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dwn State|#Active/Received/Accepted/Damped...
    I-ROOT (managed by Netnod)
    195.69.144.56          8674     109306    4827053       0       1     5w2d20h Established
      inet.0: 5/5/5/0
    ----------
    F-ROOT
    195.69.144.111        30132     118836     477895       0       1     5w2d20h Established
      inet.0: 2/2/2/0
    ----------
    F-ROOT 2
    195.69.144.140        30132     118736     482270       0       2     2w1d18h Established
      inet.0: 2/2/2/0
    ----------
    K-ROOT (managed by Ripe)
    195.69.144.240        25152      88934     384759       0       2     4w2d21h Established
      inet.0: 3/3/3/0
    ----------
    [etc....]

    me@router-re1> op bgpsum state down
    peer IP                              peer ASN          downtime   peer description
    ==================================================================================
    195.22.213.xxx                          xxxxx           5w2d22h   SOMEONE
    2001:7f8:1::a50x:xxxx:1                 xxxxx             23:19   SOMEONE ELSE

    me@router-re1> op bgpsum name DENIC state down
    Peer                     AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dwn State|#Active/Received/Accepted/Damped...
    DENIC
    195.69.145.160        31529     127971     479438       0       2       26:04 Active
    ----------
    DENIC
    2001:7f8:1::a503:1529:1     31529     119681     116429       0       2       25:55 Active
    ----------

Bugs
----

Maybe.

ChangeLog
---------

-   1.0 - Initial Version
-   1.1 - Added name parameter to match against the description, state parameter to match against state.
-   2011.06.17 - Make description match case insensitive
