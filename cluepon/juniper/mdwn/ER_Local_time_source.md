---
title: ER Local time source
permalink: /ER_Local_time_source/
---

Problem
-------

When the NTP daemon has no reachable peers/servers, it will become "unsynchronized", and clients will not be able to sync to the router any more. This effectively makes all clients below such a box free-floating.

Solution
--------

Optionally allow the router to be configured as a stratum 10 time source. This allows clients to fall back and continue to remain in sync with each other internally, even if the router is disconnected from its time sources.

This would be accomplished with the following configuration in ntp.conf:

` server  127.127.1.0 # local clock`
` fudge   127.127.1.0 stratum 10`

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")