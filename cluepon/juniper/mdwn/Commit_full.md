---
title: Commit full
permalink: /Commit_full/
---

In [JunOS](/JunOS "wikilink") version 5.1, Juniper introduced the concept of a partial commit. When a user executes the "commit" command, only the portions of the operating system affected by the changed configuration are updated.

The partial commit process normally results in a much faster "commit". However, sometimes a software bug can result in internal state inconsistencies. To force a "full" commit, where every configuration file is rebuilt, and every [JunOS](/JunOS "wikilink") daemon is HUP'd, use the hidden command "commit full".

Examples
--------

-   user@juniper\# commit full
